# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: invitations.spec.ts >> invitations flow >> supports list, create, and delete invitation flows
- Location: e2e/invitations.spec.ts:128:7

# Error details

```
Error: expect(locator).toBeVisible() failed

Locator: getByRole('dialog').getByRole('textbox', { name: 'Name', exact: true })
Expected: visible
Timeout: 10000ms
Error: element(s) not found

Call log:
  - Expect "toBeVisible" getByRole('dialog').getByRole('textbox', { name: 'Name', exact: true }) with timeout 10000ms
  - waiting for getByRole('dialog').getByRole('textbox', { name: 'Name', exact: true })

```

```yaml
- dialog:
  - img
  - text: Create New Invitation
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
  - button "Create Invitation"
```

# Test source

```ts
  1   | import { Keys } from "@meshery/schemas/permissions";
  2   | import { expect, test, type Page } from "@playwright/test";
  3   | import {
  4   |   createAuthenticatedShellHandlers,
  5   |   createFallbackHandlers,
  6   |   createInvitationsHandlers,
  7   |   createSharedShellHandlers,
  8   |   mockApi
  9   | } from "./helpers/network";
  10  | 
  11  | const manageInvitationsKey = {
  12  |   id: Keys.IdentityAccessManagementManageInvitations.id,
  13  |   function: Keys.IdentityAccessManagementManageInvitations.function
  14  | };
  15  | 
  16  | /**
  17  |  * TP-016 Invitation flow coverage. The invitations management page lists,
  18  |  * creates, edits, and deletes organization invitations through the
  19  |  * `/api/organizations/invitations*` RTK Query surface.
  20  |  */
  21  | 
  22  | const authenticatedUser = {
  23  |   id: "user-1",
  24  |   first_name: "Test",
  25  |   last_name: "User",
  26  |   email: "test@example.com",
  27  |   preferences: { selectedOrg: "org-1" }
  28  | };
  29  | 
  30  | const organization = {
  31  |   id: "org-1",
  32  |   name: "Test Organization",
  33  |   domain: "localhost:3000"
  34  | };
  35  | 
  36  | const existingInvitation = {
  37  |   id: "invite-1",
  38  |   owner: "user-1",
  39  |   name: "Engineering Team Invite",
  40  |   description: "Open seats for backend engineers",
  41  |   emails: ["engineer@example.com"],
  42  |   status: "enabled",
  43  |   orgId: "org-1",
  44  |   quota: 5,
  45  |   acceptedBy: [],
  46  |   roles: [],
  47  |   teams: [],
  48  |   createdAt: "2026-01-01T00:00:00Z",
  49  |   updatedAt: "2026-01-01T00:00:00Z",
  50  |   deletedAt: ""
  51  | };
  52  | 
  53  | /**
  54  |  * A field in the invitation RJSF form, addressed by its schema property.
  55  |  *
  56  |  * NOT `getByLabel`. Playwright resolves that from the `<label>` element's raw
  57  |  * text content, and RJSF appends the required marker, so an anchored
  58  |  * `getByLabel(/^Name$/i)` can never match - which is why the three fills using
  59  |  * it had been timing out for their full 60s on both master and this branch.
  60  |  *
  61  |  * `root_<property>` is RJSF's own id scheme: stable across label, casing and
  62  |  * required-marker changes, and it names the schema property the handler
  63  |  * actually reads, which is the thing worth pinning here (see
  64  |  * docs/contracts/form-schema-source-of-truth.md).
  65  |  *
  66  |  * The labels themselves are asserted separately by `expectHumanFieldLabels`
  67  |  * below. On master these render as the raw property names, because the form's
  68  |  * ui schema supplied `ui:label` (a BOOLEAN - "render the label at all") where
  69  |  * `ui:title` supplies the text. This branch fixes that, so the id locator is
  70  |  * deliberately paired with a label assertion: the ids alone would not notice
  71  |  * the titles regressing.
  72  |  */
  73  | const invitationField = (page: Page, property: string) =>
  74  |   page.getByRole("dialog").locator(`#root_${property}`);
  75  | 
  76  | /**
  77  |  * Guards the `ui:title` fix: the fields must expose human labels, not the raw
  78  |  * schema property names. Role locators are used because their accessible-name
  79  |  * computation ignores the required marker, which carries `aria-hidden`.
  80  |  */
  81  | const expectHumanFieldLabels = async (page: Page) => {
  82  |   await expect(
  83  |     page.getByRole("dialog").getByRole("textbox", { name: "Name", exact: true })
> 84  |   ).toBeVisible();
      |     ^ Error: expect(locator).toBeVisible() failed
  85  |   await expect(
  86  |     page.getByRole("dialog").getByRole("textbox", { name: "Description", exact: true })
  87  |   ).toBeVisible();
  88  | };
  89  | 
  90  | test.describe("invitations flow", () => {
  91  |   test("shows empty state and create action when no invitations exist", async ({
  92  |     page
  93  |   }) => {
  94  |     await mockApi(page, [
  95  |       ...createSharedShellHandlers({
  96  |         user: authenticatedUser,
  97  |         organizations: [organization]
  98  |       }),
  99  |       ...createAuthenticatedShellHandlers({
  100 |         orgId: "org-1",
  101 |         keys: [manageInvitationsKey]
  102 |       }),
  103 |       ...createInvitationsHandlers({ invitations: [] }),
  104 |       ...createFallbackHandlers()
  105 |     ]);
  106 | 
  107 |     await page.goto("/identity/invitations");
  108 | 
  109 |     await expect(
  110 |       page.getByRole("button", { name: /Create New Invitation/i })
  111 |     ).toBeVisible();
  112 |     await expect(page.getByText("No Invitations Found").first()).toBeVisible();
  113 |   });
  114 | 
  115 |   const invitationHandlers = () => [
  116 |     ...createSharedShellHandlers({
  117 |       user: authenticatedUser,
  118 |       organizations: [organization]
  119 |     }),
  120 |     ...createAuthenticatedShellHandlers({
  121 |       orgId: "org-1",
  122 |       keys: [manageInvitationsKey]
  123 |     }),
  124 |     ...createInvitationsHandlers({ invitations: [existingInvitation] }),
  125 |     ...createFallbackHandlers()
  126 |   ];
  127 | 
  128 |   test("supports list, create, and delete invitation flows", async ({ page }) => {
  129 |     const createdInvitationName = "Platform Team Invite";
  130 | 
  131 |     await mockApi(page, invitationHandlers());
  132 |     await page.goto("/identity/invitations");
  133 | 
  134 |     await expect(
  135 |       page.getByRole("row", { name: new RegExp(existingInvitation.name) })
  136 |     ).toBeVisible();
  137 | 
  138 |     await page.getByRole("button", { name: /Create New Invitation/i }).click();
  139 |     await expect(
  140 |       page.getByRole("dialog").getByText("Create New Invitation")
  141 |     ).toBeVisible();
  142 |     await expectHumanFieldLabels(page);
  143 |     await invitationField(page, "name").fill(createdInvitationName);
  144 |     await invitationField(page, "description").fill(
  145 |       "Invite for the platform engineering team"
  146 |     );
  147 |     await invitationField(page, "quota").fill("3");
  148 |     await page.getByRole("button", { name: "Create Invitation" }).click();
  149 | 
  150 |     const createdInvitationRow = page.getByRole("row", {
  151 |       name: new RegExp(createdInvitationName)
  152 |     });
  153 |     await expect(createdInvitationRow).toBeVisible();
  154 | 
  155 |     await createdInvitationRow.getByTitle("Delete Invitation").click();
  156 | 
  157 |     await expect(
  158 |       page.getByRole("row", { name: new RegExp(createdInvitationName) })
  159 |     ).toHaveCount(0);
  160 |     await expect(
  161 |       page.getByRole("row", { name: new RegExp(existingInvitation.name) })
  162 |     ).toBeVisible();
  163 |   });
  164 | 
  165 |   // EXPECTED TO FAIL - a live product defect, not a harness one:
  166 |   // https://github.com/layer5io/meshery-cloud/issues/5969
  167 |   //
  168 |   // `test.fail()` rather than `test.skip()` deliberately: the test still RUNS
  169 |   // and still reports, and the run goes RED the moment the defect is fixed,
  170 |   // which forces this annotation to be removed rather than outliving the bug. A
  171 |   // skip would be indistinguishable from a pass, which is the failure mode this
  172 |   // whole branch exists to remove.
  173 |   //
  174 |   // Measured: opening Edit Invitation, changing `root_name` and clicking
  175 |   // "Update Invitation" issues NO PUT at all (only the analytics beacon). The
  176 |   // dialog stays open and RJSF renders no validation message, so nothing on
  177 |   // screen tells the user why their edit did not save. That is the documented
  178 |   // dead-Save-button shape - RJSF refuses a submit naming a required property it
  179 |   // is not rendering - the same class as #5916 (Create/Edit Workspace) and #5917
  180 |   // (badge modal); see AGENTS.md "Schema-Driven Development" and
  181 |   // docs/contracts/form-schema-source-of-truth.md. Filling `emails` first does
  182 |   // not unblock it, so the missing required property is something else and
  183 |   // finding it needs the modal's schema/uiSchema pair read against the
  184 |   // construct, which is out of scope here.
```