# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: invitation-open-signup-marker.spec.ts >> open signup invitation marker >> the admin's tick reaches the wire as isDefault
- Location: e2e/invitation-open-signup-marker.spec.ts:208:7

# Error details

```
Error: the control that declares the org's open invitation

expect(locator).toBeVisible() failed

Locator: getByRole('dialog').getByRole('checkbox', { name: 'Set as open signup invitation' })
Expected: visible
Timeout: 10000ms
Error: element(s) not found

Call log:
  - the control that declares the org's open invitation getByRole('dialog').getByRole('checkbox', { name: 'Set as open signup invitation' }) with timeout 10000ms
  - waiting for getByRole('dialog').getByRole('checkbox', { name: 'Set as open signup invitation' })

```

```yaml
- dialog:
  - img
  - text: Edit Invitation
  - button:
    - img
  - paragraph:
    - text: "Invalid \"root\" object field configuration:"
    - emphasis: uiSchema order list does not contain property 'acceptanceOutcome'
    - text: .
  - text: "{\"type\":\"object\",\"additionalProperties\":false,\"description\":\"\",\"required\":[\"name\",\"status\"],\"properties\":{\"name\":{\"type\":\"string\",\"description\":\"Name of the invitation, which can be used to identify it. Required; cannot be an empty string.\",\"minLength\":1,\"maxLength\":255},\"description\":{\"type\":\"string\",\"description\":\"Description of the invitation, which can be used to provide additional context. Null or empty string means the invitation does not have a description.\",\"maxLength\":5000},\"expiresAt\":{\"type\":\"string\",\"format\":\"date-time\",\"nullable\":true,\"description\":\"Timestamp when the invitation expires, if applicable. Null or empty means the invitation does not expire.\",\"x-oapi-codegen-extra-tags\":{\"db\":\"expires_at\",\"json\":\"expiresAt\"}},\"acceptanceOutcome\":{\"description\":\"Determines what accepting this invitation does for the accepting user, for new and existing accounts alike. direct_membership (the default, and the behavior of every invitation that omits this property) adds the user to the inviting organization. child_organization creates a new child organization for the user whose parent is always the inviting organization; the child is named \\\"My Organization\\\" and follows the standard organization-creation procedure (a My Team team, a My Workspace workspace, and the accepting user as its owner with organization administrator privileges). With child_organization the invitation's roles and teams apply in the child organization, not in the inviting organization.\",\"x-go-type\":\"InvitationAcceptanceOutcome\",\"x-oapi-codegen-extra-tags\":{\"db\":\"acceptance_outcome\",\"json\":\"acceptanceOutcome\"},\"type\":\"string\",\"enum\":[\"direct_membership\",\"child_organization\"],\"default\":\"direct_membership\"},\"status\":{\"description\":\"Activation status of the invitation.\",\"x-go-type\":\"InvitationStatus\",\"type\":\"string\",\"enum\":[\"enabled\",\"disabled\"]},\"quota\":{\"type\":\"integer\",\"description\":\"Quota for the invitation; limits the number of users that can accept it. Null or empty means the invitation is unlimited.\",\"minimum\":0,\"maximum\":99999},\"isDefault\":{\"type\":\"boolean\",\"description\":\"Indicates whether the invitation is a default invitation (open invite), which can be used to assign users when signing up from fqdn or custom domain. An organization can only have one default invitation.\",\"x-oapi-codegen-extra-tags\":{\"db\":\"is_default\",\"json\":\"isDefault\"}},\"emails\":{\"type\":\"array\",\"nullable\":true,\"description\":\"Email addresses or patterns for which the invitation is valid. Null means the invitation is valid for any email address.\",\"x-go-type\":\"pq.StringArray\",\"x-go-type-import\":{\"path\":\"github.com/lib/pq\"},\"x-go-type-skip-optional-pointer\":true,\"items\":{\"type\":\"string\",\"pattern\":\"^([a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\\\.[a-z]{2,}|@[a-zA-Z0-9.-]+\\\\.[a-z]{2,})$\",\"description\":\"Exact email address or the email address pattern for which the invitation is valid.\",\"x-go-type\":\"string\"}},\"roles\":{\"type\":\"array\",\"description\":\"Roles that the user will have when accepting the invitation. Empty means the invitation does not specify any roles.\",\"x-go-type\":\"pq.StringArray\",\"x-go-type-import\":{\"path\":\"github.com/lib/pq\"},\"items\":{\"type\":\"string\",\"description\":\"Role identifier granted by this invitation on acceptance.\"}},\"teams\":{\"type\":\"array\",\"description\":\"Teams that the user will be added to when accepting the invitation. Empty means the invitation does not specify any teams.\",\"x-go-type\":\"pq.StringArray\",\"x-go-type-import\":{\"path\":\"github.com/lib/pq\"},\"items\":{\"type\":\"string\",\"description\":\"Team identifier the invited user is added to on acceptance.\"}}}}"
  - button:
    - img
  - button "Cancel"
  - button "Update Invitation"
```

# Test source

```ts
  116 |     if (
  117 |       request.method() === "PUT" &&
  118 |       pathname === `/api/organizations/invitations/${existingInvitation.id}`
  119 |     ) {
  120 |       const submitted = JSON.parse(request.postData() ?? "{}");
  121 |       return {
  122 |         body: { ...existingInvitation, ...submitted },
  123 |         status: 200,
  124 |         contentType: "application/json"
  125 |       };
  126 |     }
  127 |     return undefined;
  128 |   }
  129 | ];
  130 | 
  131 | /**
  132 |  * Opens the invitations page with the invitation API mocked, optionally
  133 |  * forcing the PUT to fail. Returns the parsed bodies of every invitation PUT
  134 |  * the page issued, so a test asserts the wire rather than the component's
  135 |  * internal state.
  136 |  *
  137 |  * The refusal handler is registered BEFORE `createInvitationsHandlers`:
  138 |  * `mockApi` takes the first handler that answers, so a failure fixture placed
  139 |  * after the working set is silently shadowed.
  140 |  */
  141 | async function openInvitationsPage(
  142 |   page: Page,
  143 |   refusal?: { status: number; body: unknown }
  144 | ): Promise<Array<Record<string, unknown>>> {
  145 |   const updates: Array<Record<string, unknown>> = [];
  146 | 
  147 |   const captureUpdates = (request: Request) => {
  148 |     if (request.method() !== "PUT") return;
  149 |     if (!new URL(request.url()).pathname.startsWith("/api/organizations/invitations"))
  150 |       return;
  151 |     const body = request.postData();
  152 |     if (body) {
  153 |       updates.push(JSON.parse(body));
  154 |     }
  155 |   };
  156 | 
  157 |   await mockApi(page, [
  158 |     request => {
  159 |       captureUpdates(request);
  160 |       if (!refusal) return undefined;
  161 |       if (request.method() !== "PUT") return undefined;
  162 |       if (!new URL(request.url()).pathname.startsWith("/api/organizations/invitations")) {
  163 |         return undefined;
  164 |       }
  165 |       return {
  166 |         body: refusal.body,
  167 |         status: refusal.status,
  168 |         contentType: "application/json"
  169 |       };
  170 |     },
  171 |     ...createSharedShellHandlers({
  172 |       user: authenticatedUser,
  173 |       organizations: [organization]
  174 |     }),
  175 |     ...createAuthenticatedShellHandlers({ orgId: "org-1", keys: [manageInvitationsKey] }),
  176 |     ...invitationHandlers,
  177 |     ...createFallbackHandlers()
  178 |   ]);
  179 | 
  180 |   await page.goto("/identity/invitations");
  181 |   return updates;
  182 | }
  183 | 
  184 | /**
  185 |  * The open-signup checkbox.
  186 |  *
  187 |  * Addressed by the sentence the admin actually reads. It used to be addressed
  188 |  * by its PROPERTY NAME, because the ui schema carried that sentence under
  189 |  * `ui:label`, which RJSF reads as a BOOLEAN ("render the label at all") rather
  190 |  * than as label text - so the string was merely truthy and the control the
  191 |  * whole open-signup feature hangs on rendered as a checkbox labelled
  192 |  * `isDefault`. `ui:title` now supplies the words (#6001), and this locator
  193 |  * moved with the repair, exactly as the note it replaces asked for.
  194 |  */
  195 | const openSignupCheckbox = (modal: ReturnType<Page["getByRole"]>) =>
  196 |   modal.getByRole("checkbox", { name: "Set as open signup invitation" });
  197 | 
  198 | async function openEditModal(page: Page) {
  199 |   const row = page.getByRole("row", { name: new RegExp(existingInvitation.name) });
  200 |   await expect(row).toBeVisible();
  201 |   await row.getByTitle("Edit Invitation").click();
  202 |   const modal = page.getByRole("dialog");
  203 |   await expect(modal.getByText("Edit Invitation")).toBeVisible();
  204 |   return modal;
  205 | }
  206 | 
  207 | test.describe("open signup invitation marker", () => {
  208 |   test("the admin's tick reaches the wire as isDefault", async ({ page }) => {
  209 |     const updates = await openInvitationsPage(page);
  210 |     const modal = await openEditModal(page);
  211 | 
  212 |     const marker = openSignupCheckbox(modal);
  213 |     await expect(
  214 |       marker,
  215 |       "the control that declares the org's open invitation"
> 216 |     ).toBeVisible();
      |       ^ Error: the control that declares the org's open invitation
  217 |     await expect(marker).not.toBeChecked();
  218 |     await marker.check();
  219 |     await page.screenshot({ path: shot("open-signup-control-ticked"), fullPage: true });
  220 | 
  221 |     await modal.getByRole("button", { name: "Update Invitation" }).click();
  222 | 
  223 |     await expect.poll(() => updates.length).toBe(1);
  224 |     expect(
  225 |       updates[0].isDefault,
  226 |       "the value the server persists must be the one the admin ticked"
  227 |     ).toBe(true);
  228 |   });
  229 | 
  230 |   test("a refused marker change reports the server's reason, not 'Unknown error'", async ({
  231 |     page
  232 |   }) => {
  233 |     const refusal = "Not permitted to change the organization's open signup invitation";
  234 |     const updates = await openInvitationsPage(page, {
  235 |       status: 403,
  236 |       body: { error: refusal }
  237 |     });
  238 |     const modal = await openEditModal(page);
  239 | 
  240 |     await openSignupCheckbox(modal).check();
  241 |     await modal.getByRole("button", { name: "Update Invitation" }).click();
  242 | 
  243 |     await expect.poll(() => updates.length).toBe(1);
  244 |     await expect(page.getByText(refusal)).toBeVisible();
  245 |     await expect(
  246 |       page.getByText("Unknown error"),
  247 |       "the refusal carries its text under `error`, which the modal used to ignore"
  248 |     ).toHaveCount(0);
  249 |     await page.screenshot({ path: shot("marker-refusal-toast"), fullPage: true });
  250 |   });
  251 | 
  252 |   test("an invitation deleted under a stale edit modal reports the 404's text", async ({
  253 |     page
  254 |   }) => {
  255 |     const notFound = "invitation not found";
  256 |     const updates = await openInvitationsPage(page, {
  257 |       status: 404,
  258 |       body: { error: notFound }
  259 |     });
  260 |     const modal = await openEditModal(page);
  261 | 
  262 |     await modal.getByLabel(/^Description$/i).fill("edited from a stale modal");
  263 |     await modal.getByRole("button", { name: "Update Invitation" }).click();
  264 | 
  265 |     await expect.poll(() => updates.length).toBe(1);
  266 |     await expect(page.getByText(notFound)).toBeVisible();
  267 |     await expect(page.getByText("Unknown error")).toHaveCount(0);
  268 |     await page.screenshot({ path: shot("stale-modal-404-toast"), fullPage: true });
  269 |   });
  270 | });
  271 | 
```