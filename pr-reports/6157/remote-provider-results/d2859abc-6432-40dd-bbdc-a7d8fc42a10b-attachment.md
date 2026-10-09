# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: design-import.spec.ts >> import design — canonical form contract at the product surface >> URL Import branch posts { save, url, patternData.name }
- Location: e2e/design-import.spec.ts:118:7

# Error details

```
Error: expect(locator).toBeVisible() failed

Locator: getByRole('dialog').filter({ hasText: 'Import Design' }).getByRole('textbox', { name: 'Design file name', exact: true })
Expected: visible
Timeout: 10000ms
Error: element(s) not found

Call log:
  - Expect "toBeVisible" getByRole('dialog').filter({ hasText: 'Import Design' }).getByRole('textbox', { name: 'Design file name', exact: true }) with timeout 10000ms
  - waiting for getByRole('dialog').filter({ hasText: 'Import Design' }).getByRole('textbox', { name: 'Design file name', exact: true })

```

```yaml
- dialog:
  - img
  - text: Import Design
  - button:
    - img
  - text: Design Name
  - textbox "Design Name"
  - 'button "Design Name: Provide a name for your design. This name will help you identify the design later. You can also change the name of your design after importing it."':
    - img
  - text: Upload method
  - radiogroup:
    - radio "File Upload"
    - text: File Upload
    - radio "URL Import" [checked]
    - text: URL Import
  - text: URL
  - textbox "URL"
  - 'button "URL: A direct URL to a single file, for example: https://raw.githubusercontent.com/your-design-file.yaml. Ensure the resource is in a supported format: Kubernetes Manifest, Helm Chart, Docker Compose, or Meshery Design."':
    - img
  - button "Cancel"
  - button "Import"
```

# Test source

```ts
  13  |  *
  14  |  * This spec closes the loop at the level a user experiences: it opens the real
  15  |  * modal, drives BOTH uploadType branches, and asserts the exact body that
  16  |  * reaches `POST /api/content/patterns` — which is what would silently become
  17  |  * `{save: true, patternData: {name: undefined}}` if the formData shape ever
  18  |  * drifted from the schema (the failure mode #5863 describes for filters).
  19  |  */
  20  | 
  21  | import { expect, test } from "@playwright/test";
  22  | import path from "node:path";
  23  | import {
  24  |   createAuthenticatedShellHandlers,
  25  |   createDesignsHandlers,
  26  |   createFallbackHandlers,
  27  |   createPermissionKeysHandler,
  28  |   createSharedShellHandlers,
  29  |   mockApi
  30  | } from "./helpers/network";
  31  | 
  32  | const MY_DESIGNS_URL = "/catalog/content/my-designs";
  33  | 
  34  | /**
  35  |  * Where the reviewer-facing screenshots land. Defaults inside `test-results/`
  36  |  * (gitignored, cleaned with the rest of the run); override with
  37  |  * `DESIGN_IMPORT_EVIDENCE_DIR` to collect them somewhere durable.
  38  |  */
  39  | const EVIDENCE_DIR =
  40  |   process.env.DESIGN_IMPORT_EVIDENCE_DIR ??
  41  |   path.resolve(__dirname, "..", "test-results", "design-import-evidence");
  42  | 
  43  | const shot = (name: string) => path.join(EVIDENCE_DIR, `${name}.png`);
  44  | 
  45  | const authenticatedUser = {
  46  |   id: "user-1",
  47  |   first_name: "Test",
  48  |   last_name: "User",
  49  |   email: "test@example.com",
  50  |   preferences: { selectedOrg: "org-1" }
  51  | };
  52  | 
  53  | const organization = {
  54  |   id: "org-1",
  55  |   name: "Test Organization",
  56  |   domain: "localhost:3000"
  57  | };
  58  | 
  59  | const DESIGN_YAML = "apiVersion: v1\nkind: Namespace\nmetadata:\n  name: import-demo\n";
  60  | 
  61  | /**
  62  |  * Boots the My Designs page with an empty design list and the one permission
  63  |  * key the page gates on, and returns the array the upsert bodies land in.
  64  |  */
  65  | async function openMyDesigns(page: import("@playwright/test").Page) {
  66  |   const upsertBodies: Array<Record<string, unknown>> = [];
  67  | 
  68  |   await mockApi(page, [
  69  |     ...createSharedShellHandlers({
  70  |       user: authenticatedUser,
  71  |       organizations: [organization]
  72  |     }),
  73  |     ...createPermissionKeysHandler(["VIEW_CATALOG", "VIEW_DESIGNS"]),
  74  |     ...createAuthenticatedShellHandlers({ orgId: "org-1", skipKeys: true }),
  75  |     // Records the design-import request body, then falls through (returns
  76  |     // undefined) so `createDesignsHandlers` below still answers it.
  77  |     request => {
  78  |       if (
  79  |         request.method() === "POST" &&
  80  |         new URL(request.url()).pathname === "/api/content/patterns"
  81  |       ) {
  82  |         upsertBodies.push(JSON.parse(request.postData() ?? "{}"));
  83  |       }
  84  |       return undefined;
  85  |     },
  86  |     ...createDesignsHandlers({ patterns: [] }),
  87  |     ...createFallbackHandlers()
  88  |   ]);
  89  | 
  90  |   await page.goto(MY_DESIGNS_URL, { waitUntil: "domcontentloaded" });
  91  |   await expect(page.getByRole("button", { name: "Import" })).toBeVisible();
  92  | 
  93  |   return upsertBodies;
  94  | }
  95  | 
  96  | /** The design-name field itself, never its information affordance. */
  97  | const designFileName = (modal: import("@playwright/test").Locator) =>
  98  |   modal.getByRole("textbox", { name: "Design file name", exact: true });
  99  | 
  100 | /** Opens the Import Design modal and waits for the RJSF chunk to render. */
  101 | async function openImportModal(page: import("@playwright/test").Page) {
  102 |   await page.getByRole("button", { name: "Import" }).click();
  103 |   const modal = page.getByRole("dialog").filter({ hasText: "Import Design" });
  104 |   await expect(modal).toBeVisible();
  105 |   // The RJSF form is code-split (`dynamic(() => import("../rjsf"))`); wait for
  106 |   // a real field rather than the spinner.
  107 |   //
  108 |   // By ROLE, not `getByLabel`: every RJSF field now carries an information
  109 |   // affordance whose accessible name is `<label>: <description>`, and
  110 |   // `getByLabel` matches a name by SUBSTRING - so a bare `getByLabel("Design
  111 |   // file name")` resolves to the input AND its help button (strict mode
  112 |   // violation). The button is a `button`; the field is a `textbox`.
> 113 |   await expect(designFileName(modal)).toBeVisible();
      |                                       ^ Error: expect(locator).toBeVisible() failed
  114 |   return modal;
  115 | }
  116 | 
  117 | test.describe("import design — canonical form contract at the product surface", () => {
  118 |   test("URL Import branch posts { save, url, patternData.name }", async ({ page }) => {
  119 |     const upsertBodies = await openMyDesigns(page);
  120 |     const modal = await openImportModal(page);
  121 | 
  122 |     // `uploadType` defaults to "URL Import" in the canonical schema, so the URL
  123 |     // branch is what a user sees first.
  124 |     await expect(modal.getByRole("radio", { name: "URL Import" })).toBeChecked();
  125 |     await expect(modal.getByRole("textbox", { name: "URL", exact: true })).toBeVisible();
  126 |     await modal.screenshot({ path: shot("import-modal-url-branch") });
  127 | 
  128 |     await designFileName(modal).fill("Imported From URL");
  129 |     await modal
  130 |       .getByRole("textbox", { name: "URL", exact: true })
  131 |       .fill("https://raw.githubusercontent.com/example/design.yaml");
  132 | 
  133 |     await modal.screenshot({ path: shot("import-modal-url-branch-filled") });
  134 | 
  135 |     await modal.getByRole("button", { name: "Import", exact: true }).click();
  136 | 
  137 |     await expect.poll(() => upsertBodies.length).toBe(1);
  138 |     expect(upsertBodies[0]).toEqual({
  139 |       save: true,
  140 |       url: "https://raw.githubusercontent.com/example/design.yaml",
  141 |       patternData: { name: "Imported From URL" }
  142 |     });
  143 | 
  144 |     // The user-visible outcome: success toast, and the imported design now in
  145 |     // the list — which only happens if `patternData.name` actually carried a
  146 |     // value through the form -> handler -> request-body chain.
  147 |     await expect(page.getByText("Design imported")).toBeVisible();
  148 |     await expect(page.getByText("Imported From URL").first()).toBeVisible();
  149 |     await page.screenshot({ path: shot("import-url-success-toast") });
  150 |   });
  151 | 
  152 |   test("File Upload branch posts { save, patternData.name, patternData.patternFile } with the data-url prefix stripped", async ({
  153 |     page
  154 |   }) => {
  155 |     const upsertBodies = await openMyDesigns(page);
  156 |     const modal = await openImportModal(page);
  157 | 
  158 |     // Switching the discriminator swaps the conditional branch: the URL text
  159 |     // field is replaced by the RJSF FileWidget.
  160 |     await modal.getByRole("radio", { name: "File Upload" }).check();
  161 |     await expect(modal.getByRole("textbox", { name: "URL", exact: true })).toHaveCount(0);
  162 | 
  163 |     const fileInput = modal.locator('input[type="file"]');
  164 |     await expect(fileInput).toBeAttached();
  165 |     await modal.screenshot({ path: shot("import-modal-file-branch") });
  166 | 
  167 |     await designFileName(modal).fill("Imported From File");
  168 |     await fileInput.setInputFiles({
  169 |       name: "namespace.yaml",
  170 |       mimeType: "application/x-yaml",
  171 |       buffer: Buffer.from(DESIGN_YAML, "utf8")
  172 |     });
  173 | 
  174 |     await modal.screenshot({ path: shot("import-modal-file-branch-filled") });
  175 | 
  176 |     await modal.getByRole("button", { name: "Import", exact: true }).click();
  177 | 
  178 |     await expect.poll(() => upsertBodies.length).toBe(1);
  179 |     // `patternFile` is the DECODED file, proving `getDecodedFile` stripped the
  180 |     // `data:<mime>;base64,` prefix the RJSF `format: data-url` widget emits.
  181 |     expect(upsertBodies[0]).toEqual({
  182 |       save: true,
  183 |       patternData: {
  184 |         name: "Imported From File",
  185 |         patternFile: DESIGN_YAML
  186 |       }
  187 |     });
  188 | 
  189 |     await expect(page.getByText("Design imported")).toBeVisible();
  190 |     await expect(page.getByText("Imported From File").first()).toBeVisible();
  191 |     await page.screenshot({ path: shot("import-file-success-toast") });
  192 |   });
  193 | });
  194 | 
```