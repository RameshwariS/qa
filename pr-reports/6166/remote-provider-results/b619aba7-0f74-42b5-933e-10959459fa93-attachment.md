# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: github-connection-wizard.spec.ts >> GitHub connection wizard - entry >> TC-017-003 the Connect button targets a GitHub App install URL for THIS environment
- Location: e2e/github-connection-wizard.spec.ts:120:7

# Error details

```
Error: expect(received).toHaveLength(expected)

Expected length: 1
Received length: 0
Received array:  []

Call Log:
- Timeout 10000ms exceeded while waiting on the predicate
```

# Page snapshot

```yaml
- generic [ref=f2e2]:
  - generic [ref=f2e3]:
    - link "Skip to content" [ref=f2e4] [cursor=pointer]:
      - /url: "#start-of-content"
    - banner [ref=f2e6]
  - main [ref=f2e9]:
    - generic [ref=f2e10]:
      - generic [ref=f2e12]:
        - img "Meshery Dev logo" [ref=f2e14]
        - paragraph [ref=f2e15]:
          - text: Sign in to
          - strong [ref=f2e16]: GitHub
          - text: to continue to
          - strong [ref=f2e17]: Meshery Dev
      - generic [ref=f2e18]:
        - generic [ref=f2e19]:
          - generic [ref=f2e20]:
            - generic [ref=f2e21]: Username or email address
            - textbox "Username or email address" [active] [ref=f2e22]
          - generic [ref=f2e23]:
            - generic [ref=f2e24]: Password
            - textbox "Password" [ref=f2e25]
            - link "Forgot password?" [ref=f2e26] [cursor=pointer]:
              - /url: /password_reset
          - button "Sign in" [ref=f2e28] [cursor=pointer]
        - generic [ref=f2e29]:
          - generic [ref=f2e30]: or
          - button "Continue with Google" [ref=f2e33] [cursor=pointer]
          - button "Continue with Apple" [ref=f2e37] [cursor=pointer]
      - generic [ref=f2e40]:
        - paragraph [ref=f2e42]:
          - text: New to GitHub?
          - link "Create an account" [ref=f2e43] [cursor=pointer]:
            - /url: /signup?return_to=%2Fapps%2Fmeshery-dev%2Finstallations%2Fnew&source=login
        - paragraph [ref=f2e45]:
          - button "Sign in with a passkey" [ref=f2e46] [cursor=pointer]
  - contentinfo [ref=f2e49]:
    - list [ref=f2e50]:
      - listitem [ref=f2e51]:
        - link "Terms" [ref=f2e52] [cursor=pointer]:
          - /url: https://docs.github.com/site-policy/github-terms/github-terms-of-service
      - listitem [ref=f2e53]:
        - link "Privacy" [ref=f2e54] [cursor=pointer]:
          - /url: https://docs.github.com/site-policy/privacy-policies/github-privacy-statement
      - listitem [ref=f2e55]:
        - link "Docs" [ref=f2e56] [cursor=pointer]:
          - /url: https://docs.github.com
      - listitem [ref=f2e57]:
        - link "Contact GitHub Support" [ref=f2e58] [cursor=pointer]:
          - /url: https://support.github.com
      - listitem [ref=f2e59]:
        - button "Manage cookies" [ref=f2e61] [cursor=pointer]
      - listitem [ref=f2e62]:
        - button "Do not share my personal information" [ref=f2e64] [cursor=pointer]
```

# Test source

```ts
  51  | 
  52  | /**
  53  |  * The wizard is rendered inside the authenticated AppShell, which supplies the
  54  |  * `profileData` prop the route orchestration reads for the fallback URL id.
  55  |  * Every spec needs that shell, so build it once.
  56  |  *
  57  |  * Ordering matters: `createAuthenticatedShellHandlers` answers
  58  |  * `GET /api/integrations/connections` with a hardcoded empty list for the navbar
  59  |  * connection badges. It must come AFTER `createConnectionsHandlers`, or it
  60  |  * shadows the fixture and the wizard can never see an existing GitHub
  61  |  * connection - which silently turns every "already connected" case into a
  62  |  * "first install" case.
  63  |  */
  64  | const shellHandlers = (connections: Array<Record<string, unknown>>) => [
  65  |   ...createSharedShellHandlers({
  66  |     user: authenticatedUser,
  67  |     organizations: [organization]
  68  |   }),
  69  |   ...createConnectionsHandlers({ connections }),
  70  |   ...createAuthenticatedShellHandlers({
  71  |     orgId: "org-1",
  72  |     keys: [viewConnectionsKey]
  73  |   }),
  74  |   ...createFallbackHandlers()
  75  | ];
  76  | 
  77  | /**
  78  |  * `npm run dev` starts the UI with an empty STRIPE_PUBLIC_KEY, so Stripe.js
  79  |  * logs one unconditional error on every authenticated page. It is environmental
  80  |  * and unrelated to the wizard; filter it so these specs fail only on errors the
  81  |  * wizard itself raises.
  82  |  */
  83  | const STRIPE_DEV_NOISE = /Please call Stripe\(\) with your publishable key/;
  84  | const wizardPageErrors = (errors: string[]) =>
  85  |   errors.filter(message => !STRIPE_DEV_NOISE.test(message));
  86  | 
  87  | test.describe("GitHub connection wizard - entry", () => {
  88  |   test("TC-017-001 direct URL /connect/github/new renders step 1 with the Connect action", async ({
  89  |     page
  90  |   }) => {
  91  |     await mockApi(page, [...createGithubWizardHandlers(), ...shellHandlers([])]);
  92  | 
  93  |     await page.goto("/connect/github/new");
  94  | 
  95  |     // The stepper rail lists all seven steps; step 1 is the GitHub App connect.
  96  |     await expect(page.getByText("Connect GitHub App").first()).toBeVisible();
  97  |     await expect(page.getByRole("button", { name: /^Connect$/ }).first()).toBeVisible();
  98  |   });
  99  | 
  100 |   test("TC-017-002 wizard shows every stepper stage, not just the active one", async ({
  101 |     page
  102 |   }) => {
  103 |     await mockApi(page, [...createGithubWizardHandlers(), ...shellHandlers([])]);
  104 | 
  105 |     await page.goto("/connect/github/new");
  106 | 
  107 |     for (const label of [
  108 |       "Connect GitHub App",
  109 |       "Select Repositories",
  110 |       "Import Designs",
  111 |       "Finish"
  112 |     ]) {
  113 |       await expect(page.getByText(label).first()).toBeVisible();
  114 |     }
  115 |     // "Enable Snapshots" labels three consecutive steps, distinguished by their
  116 |     // secondary line.
  117 |     await expect(page.getByText("(Configure Secrets)").first()).toBeVisible();
  118 |   });
  119 | 
  120 |   test("TC-017-003 the Connect button targets a GitHub App install URL for THIS environment", async ({
  121 |     page
  122 |   }) => {
  123 |     // `window.open(GITHUB_APP_URL, "_self")` is the handoff. Stub it before any
  124 |     // app code runs and record the target rather than following it - github.com
  125 |     // is out of the test's reach.
  126 |     await page.addInitScript(() => {
  127 |       (window as unknown as { __openedUrls: string[] }).__openedUrls = [];
  128 |       window.open = ((url?: string | URL) => {
  129 |         (window as unknown as { __openedUrls: string[] }).__openedUrls.push(
  130 |           String(url ?? "")
  131 |         );
  132 |         return null;
  133 |       }) as typeof window.open;
  134 |     });
  135 | 
  136 |     await mockApi(page, [...createGithubWizardHandlers(), ...shellHandlers([])]);
  137 | 
  138 |     await page.goto("/connect/github/new");
  139 | 
  140 |     await page
  141 |       .getByRole("button", { name: /^Connect$/ })
  142 |       .first()
  143 |       .click();
  144 | 
  145 |     await expect
  146 |       .poll(() =>
  147 |         page.evaluate(
  148 |           () => (window as unknown as { __openedUrls: string[] }).__openedUrls
  149 |         )
  150 |       )
> 151 |       .toHaveLength(1);
      |        ^ Error: expect(received).toHaveLength(expected)
  152 | 
  153 |     const [opened] = await page.evaluate(
  154 |       () => (window as unknown as { __openedUrls: string[] }).__openedUrls
  155 |     );
  156 | 
  157 |     expect(opened).toMatch(/^https:\/\/github\.com\/apps\/[a-z-]+\/installations\/new$/);
  158 | 
  159 |     // The dev server runs with NODE_ENV=development, so this must be the
  160 |     // meshery-dev app - NOT the production `meshery` app. `make ui-build`
  161 |     // builds with NODE_ENV=production (deliberately, commit 2c46dec1efe) and
  162 |     // used to bake the production app URL into a local build, which makes the
  163 |     // installation handoff unreturnable locally: the production app's callback
  164 |     // points at cloud.layer5.io. The app URL now keys off the deployment
  165 |     // target, not the build mode.
  166 |     expect(opened).not.toContain("/apps/meshery/installations/new");
  167 |   });
  168 | });
  169 | 
  170 | test.describe("GitHub connection wizard - installation return leg", () => {
  171 |   test("TC-017-004 returning with githubLogin=true advances past step 1 to repository selection", async ({
  172 |     page
  173 |   }) => {
  174 |     await mockApi(page, [
  175 |       ...createGithubWizardHandlers({
  176 |         repositories: [
  177 |           { name: "sample-repo", owner: { login: "octo-org" } },
  178 |           { name: "other-repo", owner: { login: "octo-org" } }
  179 |         ]
  180 |       }),
  181 |       ...shellHandlers([githubConnectionFixture()])
  182 |     ]);
  183 | 
  184 |     await page.goto("/connect/github/reconfigure/gh-conn-1?githubLogin=true&id=9911");
  185 | 
  186 |     await expect(
  187 |       page.getByLabel("Select a repository", { exact: true }).first()
  188 |     ).toBeVisible();
  189 |   });
  190 | 
  191 |   test("TC-017-005 repository list is fetched with the installation id from the URL", async ({
  192 |     page
  193 |   }) => {
  194 |     const seen: string[] = [];
  195 |     page.on("request", req => {
  196 |       const u = new URL(req.url());
  197 |       if (u.pathname === "/api/integrations/github") {
  198 |         seen.push(u.searchParams.get("id") ?? "");
  199 |       }
  200 |     });
  201 | 
  202 |     await mockApi(page, [
  203 |       ...createGithubWizardHandlers(),
  204 |       ...shellHandlers([githubConnectionFixture()])
  205 |     ]);
  206 | 
  207 |     await page.goto("/connect/github/reconfigure/gh-conn-1?githubLogin=true&id=9911");
  208 |     await expect(
  209 |       page.getByLabel("Select a repository", { exact: true }).first()
  210 |     ).toBeVisible();
  211 | 
  212 |     expect(seen.length).toBeGreaterThan(0);
  213 |     expect(seen.every(id => id === "9911")).toBe(true);
  214 |   });
  215 | 
  216 |   test("TC-017-006 a single available repository is auto-selected", async ({ page }) => {
  217 |     await mockApi(page, [
  218 |       ...createGithubWizardHandlers({
  219 |         repositories: [{ name: "only-repo", owner: { login: "octo-org" } }]
  220 |       }),
  221 |       ...shellHandlers([githubConnectionFixture()])
  222 |     ]);
  223 | 
  224 |     await page.goto("/connect/github/reconfigure/gh-conn-1?githubLogin=true&id=9911");
  225 | 
  226 |     await expect(page.getByText("octo-org/only-repo").first()).toBeVisible();
  227 |   });
  228 | });
  229 | 
  230 | test.describe("GitHub connection wizard - metadata shape compatibility", () => {
  231 |   /**
  232 |    * The `installationId` / `installation_id` fallback exists because the wire
  233 |    * shape changed once. `readGithubMetadata` has unit coverage; these two cases
  234 |    * prove the fallback actually reaches the browser - the redirect that carries
  235 |    * the installation id into the stepper is built from it in
  236 |    * `components/connect/index.tsx`.
  237 |    */
  238 |   for (const shape of ["camel", "snake"] as const) {
  239 |     test(`TC-017-007-${shape} /connect/github/new on an existing connection carries the installation id into the reconfigure URL (${shape}Case metadata)`, async ({
  240 |       page
  241 |     }) => {
  242 |       await mockApi(page, [
  243 |         ...createGithubWizardHandlers(),
  244 |         ...shellHandlers([
  245 |           githubConnectionFixture({ metadataShape: shape, installationId: "5150" })
  246 |         ])
  247 |       ]);
  248 | 
  249 |       await page.goto("/connect/github/new");
  250 | 
  251 |       // The wizard detects the existing connection and re-routes to the
```