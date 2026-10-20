# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: branded-tenant-no-provider-brand.spec.ts >> branded tenant host: no provider brand on any reachable page >> /identity/organizations names nothing of the provider
- Location: e2e/branded-tenant-no-provider-brand.spec.ts:134:9

# Error details

```
Error: provider brand on /identity/organizations for a branded tenant

expect(received).toEqual(expected) // deep equality

- Expected  - 1
+ Received  + 3

- Array []
+ Array [
+   "link: https://docs.meshery.io/reference/references/permissions/",
+ ]
```

# Page snapshot

```yaml
- generic [active] [ref=e1]:
  - generic [ref=e3]:
    - banner [ref=e4]:
      - generic [ref=e7]:
        - link [ref=e9] [cursor=pointer]:
          - /url: /
          - paragraph [ref=e10]: Blowhorn
        - generic [ref=e13]:
          - combobox [ref=e14] [cursor=pointer]:
            - generic [ref=e41]: Blowhorn
          - textbox [aria-hidden]: org-blowhorn
          - group [aria-hidden]
        - generic [ref=e44] [cursor=pointer]
        - generic [ref=e57]:
          - button [ref=e58] [cursor=pointer]
          - generic "Profile" [ref=e61] [cursor=pointer]
    - generic [ref=e65]:
      - heading "Organizations" [level=1] [ref=e66]
      - tablist "scrollable auto tabs example" [ref=e69]:
        - tab "Overview" [ref=e70] [cursor=pointer]
        - tab "Users" [disabled] [ref=e74]
        - tab "Teams" [disabled] [ref=e78]
        - tab "Roles" [disabled] [ref=e83]
        - tab "Organizations" [disabled] [selected] [ref=e87]
        - tab "Invitations" [disabled] [ref=e95]
        - tab "Requests" [disabled] [ref=e100]
    - main [ref=e109]:
      - heading "Oops! It seems like you don't have the necessary permissions to view this page." [level=4] [ref=e111]
      - generic [ref=e113]:
        - generic [ref=e114]:
          - heading "YOUR ACCESS" [level=5] [ref=e115]
          - generic [ref=e116]:
            - paragraph [ref=e117]: Authorization Required
            - paragraph [ref=e118]: "Needs any of: View All Organizations, View Org"
            - generic [ref=e120]:
              - button "Copy key ID for View All Organizations" [ref=e121] [cursor=pointer]
              - paragraph [ref=e124]: View All Organizations
            - paragraph [ref=e126]: See all organizations within a Cloud deployment. See all organizations, teams, and users.
            - generic [ref=e127]:
              - button "Copy key ID for View Org" [ref=e128] [cursor=pointer]
              - paragraph [ref=e131]: View Org
            - paragraph [ref=e133]: See only organizations to which you are a member. See all other members within your membership teams.
            - generic [ref=e135]:
              - generic [ref=e136]:
                - generic [ref=e137]: Identity & Access Management
                - generic [ref=e139]: Organizations
              - link [ref=e141] [cursor=pointer]:
                - /url: https://docs.meshery.io/reference/references/permissions/
            - generic [ref=e146]:
              - generic [ref=e147]:
                - paragraph [ref=e152]: User
                - paragraph [ref=e153]: avery@example.com
              - generic [ref=e154]:
                - paragraph [ref=e161]: Org
                - paragraph [ref=e162]: Blowhorn
              - generic [ref=e163]:
                - paragraph [ref=e168]: Role(s)
                - paragraph [ref=e169]: None
            - paragraph [ref=e170]: Seeing this message in error? Contact your Admins to request access.
        - separator [ref=e171]
        - generic [ref=e172]:
          - heading "YOUR OPTIONS" [level=5] [ref=e173]
          - generic [ref=e174]:
            - paragraph [ref=e175]: To view the content of this page, switch to an organization where you have the required access using the ‘Switch Organization’ field below.
            - group [ref=e176]:
              - heading "Switch Organization" [level=6] [ref=e177]
              - paragraph [ref=e179]: You do not have the required permission in any of your organizations.
      - generic [ref=e180]:
        - link "Return to Dashboard" [ref=e181] [cursor=pointer]:
          - /url: /
        - button "Contact Us" [ref=e182] [cursor=pointer]
    - navigation [ref=e183]:
      - heading [level=6] [ref=e185]:
        - text: Active Users
        - paragraph [ref=e186]: (0)
    - generic:
      - button "Feedback" [ref=e189] [cursor=pointer]
      - generic [ref=e191]:
        - generic [ref=e192]:
          - paragraph [ref=e197]: Feedback
          - generic [ref=e204] [cursor=pointer]
        - generic [ref=e209]:
          - generic [ref=e210]:
            - button [ref=e211] [cursor=pointer]:
              - paragraph [ref=e215]: Issue
            - button [ref=e216] [cursor=pointer]:
              - paragraph [ref=e224]: Suggestion
            - button [ref=e225] [cursor=pointer]:
              - paragraph [ref=e232]: Meet Request
          - textbox "I’m having an issue with..." [ref=e234]
        - generic [ref=e236]:
          - generic [ref=e237]:
            - checkbox [ref=e239] [cursor=pointer]
            - paragraph [ref=e242]: We may email you for more information or updates
          - button "Send" [disabled]
  - alert [ref=e243]
```

# Test source

```ts
  70  | };
  71  | 
  72  | const user = {
  73  |   id: "user-1",
  74  |   first_name: "Avery",
  75  |   last_name: "Tenant",
  76  |   email: "avery@example.com",
  77  |   preferences: { selectedOrg: "org-child" }
  78  | };
  79  | 
  80  | // The pages a new tenant user can reach from sign-up on: the dashboard and
  81  | // onboarding, their account and settings, and every error surface.
  82  | const PAGES = [
  83  |   "/dashboard",
  84  |   "/account",
  85  |   "/identity/organizations",
  86  |   "/support",
  87  |   "/error",
  88  |   "/404",
  89  |   "/settings"
  90  | ];
  91  | 
  92  | // Known leaks that live outside this repository, each with its tracking issue.
  93  | // A page listed here is EXPECTED to fail (test.fail): the day the leak is fixed
  94  | // the test starts passing, Playwright reports that as an error, and the entry
  95  | // must be removed. Nothing here is skipped.
  96  | const KNOWN_EXTERNAL_LEAKS: Record<string, string> = {
  97  |   // Sistent's PermissionShield hard-codes the Meshery permissions docs link.
  98  |   "/identity/organizations": "https://github.com/layer5io/sistent/issues/1883"
  99  | };
  100 | 
  101 | async function providerBrandOccurrences(page: Page): Promise<string[]> {
  102 |   return page.evaluate(source => {
  103 |     const pattern = new RegExp(source, "i");
  104 |     const found: string[] = [];
  105 |     const context = (text: string) => {
  106 |       const m = text.match(pattern);
  107 |       if (!m || m.index === undefined) return text.slice(0, 120);
  108 |       return text.slice(Math.max(0, m.index - 50), m.index + 50).replace(/\s+/g, " ");
  109 |     };
  110 |     if (pattern.test(document.title)) found.push(`title: ${document.title}`);
  111 |     const text = document.body?.innerText || "";
  112 |     if (pattern.test(text)) found.push(`text: ...${context(text)}...`);
  113 |     document.querySelectorAll("link[rel~='icon']").forEach(el => {
  114 |       const href = (el as HTMLLinkElement).href;
  115 |       if (pattern.test(href)) found.push(`favicon: ${href}`);
  116 |     });
  117 |     document.querySelectorAll("img").forEach(el => {
  118 |       const img = el as HTMLImageElement;
  119 |       const visible = img.offsetParent !== null;
  120 |       if (visible && (pattern.test(img.currentSrc || img.src) || pattern.test(img.alt))) {
  121 |         found.push(`img: ${img.currentSrc || img.src} alt=${img.alt}`);
  122 |       }
  123 |     });
  124 |     document.querySelectorAll("a[href]").forEach(el => {
  125 |       const a = el as HTMLAnchorElement;
  126 |       if (a.offsetParent !== null && pattern.test(a.href)) found.push(`link: ${a.href}`);
  127 |     });
  128 |     return Array.from(new Set(found));
  129 |   }, PROVIDER_BRAND.source);
  130 | }
  131 | 
  132 | test.describe("branded tenant host: no provider brand on any reachable page", () => {
  133 |   for (const path of PAGES) {
  134 |     test(`${path} names nothing of the provider`, async ({ page }) => {
  135 |       const knownLeak = KNOWN_EXTERNAL_LEAKS[path];
  136 |       test.fail(Boolean(knownLeak), `known external leak: ${knownLeak}`);
  137 |       await page.route(`${tenantBase}/tenant-assets/**`, route =>
  138 |         route.fulfill({
  139 |           status: 200,
  140 |           contentType: "image/svg+xml",
  141 |           body: "<svg xmlns='http://www.w3.org/2000/svg'/>"
  142 |         })
  143 |       );
  144 |       await mockApi(page, [
  145 |         request => {
  146 |           const url = new URL(request.url());
  147 |           if (url.pathname === "/api/identity/orgs/by-domain") {
  148 |             return { status: 200, contentType: "application/json", body: blowhorn };
  149 |           }
  150 |           return undefined;
  151 |         },
  152 |         ...createSharedShellHandlers({ user, organizations: [child, blowhorn] }),
  153 |         ...createAuthenticatedShellHandlers({
  154 |           orgId: "org-child",
  155 |           skipKeys: true,
  156 |           preferences: { theme: blowhornTheme }
  157 |         }),
  158 |         ...createFallbackHandlers()
  159 |       ]);
  160 | 
  161 |       await page.goto(path, { waitUntil: "domcontentloaded" });
  162 |       await page
  163 |         .waitForLoadState("networkidle", { timeout: 10_000 })
  164 |         .catch(() => undefined);
  165 |       // Settle: the provider-host decision and the organization both resolve
  166 |       // after mount.
  167 |       await page.waitForTimeout(1500);
  168 | 
  169 |       const leaks = await providerBrandOccurrences(page);
> 170 |       expect(leaks, `provider brand on ${path} for a branded tenant`).toEqual([]);
      |                                                                       ^ Error: provider brand on /identity/organizations for a branded tenant
  171 |     });
  172 |   }
  173 | });
  174 | 
  175 | // The sign-up journey itself, before there is a session: the pages a new
  176 | // tenant user sees on the tenant host on the way in.
  177 | const AUTH_PAGES = [
  178 |   "/login",
  179 |   "/registration",
  180 |   "/verification",
  181 |   "/recovery",
  182 |   "/registered"
  183 | ];
  184 | 
  185 | test.describe("branded tenant host: no provider brand on the sign-up pages", () => {
  186 |   for (const path of AUTH_PAGES) {
  187 |     test(`${path} names nothing of the provider`, async ({ page }) => {
  188 |       await page.route(`${tenantBase}/tenant-assets/**`, route =>
  189 |         route.fulfill({
  190 |           status: 200,
  191 |           contentType: "image/svg+xml",
  192 |           body: "<svg xmlns='http://www.w3.org/2000/svg'/>"
  193 |         })
  194 |       );
  195 |       await mockApi(page, [
  196 |         request => {
  197 |           const url = new URL(request.url());
  198 |           if (url.pathname === "/api/identity/orgs/by-domain") {
  199 |             return { status: 200, contentType: "application/json", body: blowhorn };
  200 |           }
  201 |           return undefined;
  202 |         },
  203 |         ...createFallbackHandlers()
  204 |       ]);
  205 |       await page.goto(path, { waitUntil: "domcontentloaded" });
  206 |       await page
  207 |         .waitForLoadState("networkidle", { timeout: 10_000 })
  208 |         .catch(() => undefined);
  209 |       await page.waitForTimeout(1500);
  210 |       const leaks = await providerBrandOccurrences(page);
  211 |       expect(leaks, `provider brand on ${path} for a branded tenant`).toEqual([]);
  212 |     });
  213 |   }
  214 | });
  215 | 
  216 | // The detector is not vacuous: the same crawl on a PROVIDER host finds the
  217 | // provider's brand, so a branded-host pass means the brand is really gone.
  218 | test.describe("provider host control", () => {
  219 |   test.use({ baseURL: `http://localhost:${port}` });
  220 | 
  221 |   test("the crawl detects the provider brand on a provider host", async ({ page }) => {
  222 |     await mockApi(page, [
  223 |       ...createSharedShellHandlers({
  224 |         user,
  225 |         organizations: [{ ...blowhorn, domain: "" }]
  226 |       }),
  227 |       ...createAuthenticatedShellHandlers({ orgId: "org-blowhorn", skipKeys: true }),
  228 |       ...createFallbackHandlers()
  229 |     ]);
  230 |     await page.goto("/support", { waitUntil: "domcontentloaded" });
  231 |     await page
  232 |       .waitForLoadState("networkidle", { timeout: 10_000 })
  233 |       .catch(() => undefined);
  234 |     await page.waitForTimeout(1500);
  235 |     expect((await providerBrandOccurrences(page)).length).toBeGreaterThan(0);
  236 |   });
  237 | });
  238 | 
```