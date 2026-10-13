# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: invitations.spec.ts >> invitations flow >> updates an existing invitation
- Location: e2e/invitations.spec.ts:185:7

# Error details

```
Test timeout of 60000ms exceeded.
```

```
Error: locator.fill: Test timeout of 60000ms exceeded.
Call log:
  - waiting for getByRole('dialog').locator('#root_name')

```

# Page snapshot

```yaml
- generic [ref=e1]:
  - generic [ref=e3]:
    - banner [ref=e4]:
      - generic [ref=e7]:
        - link [ref=e9] [cursor=pointer]:
          - /url: /
          - paragraph [ref=e10]
        - generic [ref=e15]:
          - combobox [ref=e16] [cursor=pointer]:
            - generic [ref=e43]: Test Organization
          - textbox [aria-hidden]: org-1
          - group [aria-hidden]
        - generic [ref=e46] [cursor=pointer]
        - generic [ref=e59]:
          - button [ref=e60] [cursor=pointer]
          - generic [ref=e63] [cursor=pointer]
    - generic [ref=e67]:
      - heading [level=1] [ref=e68]: Invitations
      - tablist [ref=e71]:
        - tab [ref=e72] [cursor=pointer]: Overview
        - tab [disabled] [ref=e76]: Users
        - tab [disabled] [ref=e80]: Teams
        - tab [disabled] [ref=e85]: Roles
        - tab [disabled] [ref=e89]: Organizations
        - tab [selected] [ref=e97] [cursor=pointer]: Invitations
        - tab [disabled] [ref=e102]: Requests
    - generic [ref=e110]:
      - generic [ref=e111]:
        - button [ref=e113] [cursor=pointer]: Create New Invitation
        - generic [ref=e120]:
          - button [ref=e124] [cursor=pointer]
          - button [ref=e130] [cursor=pointer]
      - generic [ref=e134]:
        - grid [ref=e136]:
          - caption [ref=e137]
          - rowgroup [ref=e138]:
            - row [ref=e139]:
              - columnheader [ref=e140]:
                - button [ref=e142] [cursor=pointer]:
                  - generic [ref=e143]: Invite
              - columnheader [ref=e145]:
                - button [ref=e147] [cursor=pointer]:
                  - generic [ref=e148]: Owner
              - columnheader [ref=e150]:
                - button [ref=e152] [cursor=pointer]:
                  - generic [ref=e153]: Expires At
              - columnheader [ref=e155]:
                - button [ref=e157] [cursor=pointer]:
                  - generic [ref=e158]: Quota
              - columnheader [ref=e160]:
                - button [ref=e162] [cursor=pointer]:
                  - generic [ref=e163]: Organization
              - columnheader [ref=e165]:
                - button [ref=e167] [cursor=pointer]:
                  - generic [ref=e168]: Roles
              - columnheader [ref=e170]:
                - button [ref=e172] [cursor=pointer]:
                  - generic [ref=e173]: Teams
              - columnheader [ref=e175]:
                - button [ref=e177] [cursor=pointer]:
                  - generic [ref=e178]: Status
              - columnheader [ref=e180]:
                - generic [ref=e181]: Actions
          - rowgroup [ref=e182]:
            - row [ref=e183]:
              - gridcell [ref=e184]:
                - generic [ref=e186]:
                  - paragraph [ref=e188]: Engineering Team Invite
                  - paragraph [ref=e190]: Open seats for backend engineers
                  - generic [ref=e191]:
                    - paragraph [ref=e192]: http://localhost:3000/invitations/invite-1/accept
                    - generic [ref=e193] [cursor=pointer]
              - gridcell [ref=e196]:
                - generic [ref=e198]:
                  - generic [ref=e199] [cursor=pointer]
                  - paragraph [ref=e203]: N/A
              - gridcell [ref=e204]:
                - generic [ref=e205]: Never
              - gridcell [ref=e206]:
                - paragraph [ref=e209]: 0 / 5
              - gridcell [ref=e210]:
                - paragraph [ref=e212]: Test Organization
              - gridcell [ref=e213]
              - gridcell [ref=e214]
              - gridcell [ref=e215]:
                - generic [ref=e216]: enabled
              - gridcell [ref=e218]:
                - generic [ref=e220]:
                  - button [ref=e221] [cursor=pointer]
                  - button [ref=e224] [cursor=pointer]
        - table [ref=e228]:
          - rowgroup [ref=e229]:
            - row [ref=e230]:
              - cell [ref=e231]:
                - generic [ref=e234]:
                  - paragraph [ref=e235]: "Rows per page:"
                  - generic [ref=e236]:
                    - combobox [ref=e237] [cursor=pointer]: "10"
                    - textbox [aria-hidden]: "10"
                  - paragraph [ref=e238]: 1-1 of 1
                  - generic [ref=e239]:
                    - button [disabled]
                    - button [disabled]
    - navigation [ref=e241]:
      - heading [level=6] [ref=e243]:
        - text: Active Users
        - paragraph [ref=e244]: (0)
    - button [ref=e247] [cursor=pointer]: Feedback
    - generic [ref=e249]:
      - generic [ref=e250]:
        - paragraph [ref=e255]: Feedback
        - generic [ref=e262] [cursor=pointer]
      - generic [ref=e267]:
        - generic [ref=e268]:
          - button [ref=e269] [cursor=pointer]:
            - paragraph [ref=e273]: Issue
          - button [ref=e274] [cursor=pointer]:
            - paragraph [ref=e282]: Suggestion
          - button [ref=e283] [cursor=pointer]:
            - paragraph [ref=e290]: Meet Request
        - textbox [ref=e292]:
          - /placeholder: I’m having an issue with...
      - generic [ref=e294]:
        - generic [ref=e295]:
          - checkbox [ref=e297] [cursor=pointer]
          - paragraph [ref=e300]: We may email you for more information or updates
        - button [disabled]: Send
  - alert [ref=e301]
  - dialog [active] [ref=e304]:
    - generic [ref=e305]:
      - generic [ref=e310]: Edit Invitation
      - button [ref=e312] [cursor=pointer]
    - generic [ref=e320]:
      - paragraph [ref=e321]:
        - generic [ref=e322]:
          - text: "Invalid \"root\" object field configuration:"
          - emphasis [ref=e323]: uiSchema order list does not contain property 'acceptanceOutcome'
          - text: .
      - generic [ref=e324]: "{\"type\":\"object\",\"additionalProperties\":false,\"description\":\"\",\"required\":[\"name\",\"status\"],\"properties\":{\"name\":{\"type\":\"string\",\"description\":\"Name of the invitation, which can be used to identify it. Required; cannot be an empty string.\",\"minLength\":1,\"maxLength\":255},\"description\":{\"type\":\"string\",\"description\":\"Description of the invitation, which can be used to provide additional context. Null or empty string means the invitation does not have a description.\",\"maxLength\":5000},\"expiresAt\":{\"type\":\"string\",\"format\":\"date-time\",\"nullable\":true,\"description\":\"Timestamp when the invitation expires, if applicable. Null or empty means the invitation does not expire.\",\"x-oapi-codegen-extra-tags\":{\"db\":\"expires_at\",\"json\":\"expiresAt\"}},\"acceptanceOutcome\":{\"description\":\"Determines what accepting this invitation does for the accepting user, for new and existing accounts alike. direct_membership (the default, and the behavior of every invitation that omits this property) adds the user to the inviting organization. child_organization creates a new child organization for the user whose parent is always the inviting organization; the child is named \\\"My Organization\\\" and follows the standard organization-creation procedure (a My Team team, a My Workspace workspace, and the accepting user as its owner with organization administrator privileges). With child_organization the invitation's roles and teams apply in the child organization, not in the inviting organization.\",\"x-go-type\":\"InvitationAcceptanceOutcome\",\"x-oapi-codegen-extra-tags\":{\"db\":\"acceptance_outcome\",\"json\":\"acceptanceOutcome\"},\"type\":\"string\",\"enum\":[\"direct_membership\",\"child_organization\"],\"default\":\"direct_membership\"},\"status\":{\"description\":\"Activation status of the invitation.\",\"x-go-type\":\"InvitationStatus\",\"type\":\"string\",\"enum\":[\"enabled\",\"disabled\"]},\"quota\":{\"type\":\"integer\",\"description\":\"Quota for the invitation; limits the number of users that can accept it. Null or empty means the invitation is unlimited.\",\"minimum\":0,\"maximum\":99999},\"isDefault\":{\"type\":\"boolean\",\"description\":\"Indicates whether the invitation is a default invitation (open invite), which can be used to assign users when signing up from fqdn or custom domain. An organization can only have one default invitation.\",\"x-oapi-codegen-extra-tags\":{\"db\":\"is_default\",\"json\":\"isDefault\"}},\"emails\":{\"type\":\"array\",\"nullable\":true,\"description\":\"Email addresses or patterns for which the invitation is valid. Null means the invitation is valid for any email address.\",\"x-go-type\":\"pq.StringArray\",\"x-go-type-import\":{\"path\":\"github.com/lib/pq\"},\"x-go-type-skip-optional-pointer\":true,\"items\":{\"type\":\"string\",\"pattern\":\"^([a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\\\.[a-z]{2,}|@[a-zA-Z0-9.-]+\\\\.[a-z]{2,})$\",\"description\":\"Exact email address or the email address pattern for which the invitation is valid.\",\"x-go-type\":\"string\"}},\"roles\":{\"type\":\"array\",\"description\":\"Roles that the user will have when accepting the invitation. Empty means the invitation does not specify any roles.\",\"x-go-type\":\"pq.StringArray\",\"x-go-type-import\":{\"path\":\"github.com/lib/pq\"},\"items\":{\"type\":\"string\",\"description\":\"Role identifier granted by this invitation on acceptance.\"}},\"teams\":{\"type\":\"array\",\"description\":\"Teams that the user will be added to when accepting the invitation. Empty means the invitation does not specify any teams.\",\"x-go-type\":\"pq.StringArray\",\"x-go-type-import\":{\"path\":\"github.com/lib/pq\"},\"items\":{\"type\":\"string\",\"description\":\"Team identifier the invited user is added to on acceptance.\"}}}}"
    - generic [ref=e325]:
      - button [ref=e326] [cursor=pointer]
      - generic [ref=e329]:
        - button "Cancel" [ref=e330] [cursor=pointer]
        - button "Update Invitation" [ref=e331] [cursor=pointer]
```

# Test source

```ts
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
  185 |   test("updates an existing invitation", async ({ page }) => {
  186 |     // Pinned as an expected failure by #5991, because the form rendered its
  187 |     // fields with the raw schema property names: its ui schema supplied
  188 |     // `ui:label` (a BOOLEAN - "render the label at all") where `ui:title`
  189 |     // supplies the text, so the Edit dialog had no reachable "Name" field.
  190 |     //
  191 |     // UNPINNED here because this branch fixes that cause (#6001): the ui schema
  192 |     // now supplies `ui:title` for every field. Verified locally against a real
  193 |     // static export - with the pin still in place Playwright reported
  194 |     // "Expected to fail, but passed", which is itself a failure, so leaving it
  195 |     // would have turned the PR red for a reason absent from the diff.
  196 |     //
  197 |     // If you ever re-pin a case here, put `test.fail()` inside the body: in the
  198 |     // describe scope it is a modifier on the whole SUITE and silently marks the
  199 |     // passing siblings as expected-failures too.
  200 |     const updatedInvitationName = "Platform Admin Invite";
  201 | 
  202 |     await mockApi(page, invitationHandlers());
  203 |     await page.goto("/identity/invitations");
  204 | 
  205 |     const existingInvitationRow = page.getByRole("row", {
  206 |       name: new RegExp(existingInvitation.name)
  207 |     });
  208 |     await expect(existingInvitationRow).toBeVisible();
  209 |     await existingInvitationRow.getByTitle("Edit Invitation").click();
  210 | 
  211 |     await expect(page.getByRole("dialog").getByText("Edit Invitation")).toBeVisible();
> 212 |     await invitationField(page, "name").fill(updatedInvitationName);
      |                                         ^ Error: locator.fill: Test timeout of 60000ms exceeded.
  213 |     await invitationField(page, "description").fill(
  214 |       "Updated invite for platform administrators"
  215 |     );
  216 |     await page.getByRole("button", { name: "Update Invitation" }).click();
  217 | 
  218 |     await expect(
  219 |       page.getByRole("row", { name: new RegExp(updatedInvitationName) })
  220 |     ).toBeVisible();
  221 |   });
  222 | });
  223 | 
```