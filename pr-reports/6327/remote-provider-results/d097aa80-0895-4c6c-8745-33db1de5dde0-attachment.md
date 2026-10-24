# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: dashboard.spec.ts >> dashboard widget picker close control >> closes the panel only, leaving edit mode and its guards armed
- Location: e2e/dashboard.spec.ts:712:7

# Error details

```
Test timeout of 60000ms exceeded.
```

```
Error: locator.click: Test timeout of 60000ms exceeded.
Call log:
  - waiting for locator('header.MuiAppBar-root a[href="/"]').first()

```

# Page snapshot

```yaml
- generic [active] [ref=e1]:
  - generic [ref=e3]:
    - banner [ref=e4]:
      - generic [ref=e7]:
        - link [ref=e9] [cursor=pointer]:
          - /url: /dashboard
          - paragraph [ref=e10]:
            - img "logo" [ref=e12]
        - generic [ref=e15]:
          - combobox [ref=e16] [cursor=pointer]:
            - generic [ref=e43]: Test Organization
          - textbox [aria-hidden]: org-1
          - group [aria-hidden]
        - generic [ref=e46] [cursor=pointer]
        - generic [ref=e59]:
          - button [ref=e60] [cursor=pointer]
          - generic "Profile" [ref=e63] [cursor=pointer]
    - generic [ref=e67]:
      - generic [ref=e68]:
        - button [ref=e69] [cursor=pointer]
        - button [ref=e73] [cursor=pointer]
        - button [ref=e78] [cursor=pointer]
        - button [ref=e83] [cursor=pointer]
        - button [ref=e88] [cursor=pointer]
      - generic [ref=e95]:
        - generic [ref=e96]:
          - generic [ref=e97]:
            - button [ref=e98] [cursor=pointer]
            - button [ref=e102] [cursor=pointer]
          - generic [ref=e107]:
            - generic [ref=e108]: GETTING STARTED
            - generic [ref=e113]:
              - paragraph [ref=e114]: New here? Follow along these guided tasks to help you get the most of your account.
              - paragraph [ref=e115]:
                - generic [ref=e116] [cursor=pointer]
                - generic [ref=e118]: 0%
              - button "Start" [ref=e120] [cursor=pointer]
        - generic [ref=e130]:
          - generic [ref=e131]:
            - button [ref=e132] [cursor=pointer]
            - button [ref=e136] [cursor=pointer]
          - generic [ref=e141]:
            - generic [ref=e142]: LEARN BY VIDEO
            - generic [ref=e146]:
              - paragraph [ref=e147]: From Whiteboard to Workload. Give your brain a break from YAML. Watch Layer5 tools in-action.
              - button "Watch" [ref=e149] [cursor=pointer]
        - generic [ref=e158]:
          - generic [ref=e159]:
            - button [ref=e160] [cursor=pointer]
            - button [ref=e164] [cursor=pointer]
          - generic [ref=e169]:
            - heading "CLOUD NATIVE PLAYGROUND" [level=6] [ref=e196]
            - generic [ref=e197]:
              - paragraph [ref=e198]: Explore visual and collaborative infrastructure as code in the Meshery playground.
              - button "Request Access" [ref=e200] [cursor=pointer]
        - generic [ref=e209]:
          - generic [ref=e210]:
            - button [ref=e211] [cursor=pointer]
            - button [ref=e215] [cursor=pointer]
          - generic [ref=e219]:
            - generic [ref=e220]:
              - generic [ref=e221]:
                - img "service-mesh-pattern" [ref=e222]
                - generic [ref=e231]: CATALOG DESIGNS
              - generic [ref=e233]:
                - heading "0" [level=2] [ref=e234]
                - paragraph [ref=e235]: Designs
            - link [ref=e237] [cursor=pointer]:
              - /url: /catalog
              - button "See All" [ref=e238]
        - generic [ref=e247]:
          - generic [ref=e248]:
            - button [ref=e249] [cursor=pointer]
            - button [ref=e253] [cursor=pointer]
          - generic [ref=e258]:
            - heading "HELP CENTER" [level=6] [ref=e264]
            - list [ref=e266]:
              - listitem [ref=e267]:
                - link "Academy" [ref=e278] [cursor=pointer]:
                  - /url: /academy
                - superscript [ref=e279]
              - listitem [ref=e282]:
                - link "Docs" [ref=e293] [cursor=pointer]:
                  - /url: https://docs.layer5.io/
                - superscript [ref=e294]
              - listitem [ref=e297]:
                - link "Learn by Video" [ref=e308] [cursor=pointer]:
                  - /url: https://docs.layer5.io/videos
                - superscript [ref=e309]
              - listitem [ref=e312]:
                - link "Support Request" [ref=e323] [cursor=pointer]:
                  - /url: /support
              - listitem [ref=e324]:
                - link "Discussion Forum" [ref=e335] [cursor=pointer]:
                  - /url: https://discuss.meshery.io
                - superscript [ref=e336]
              - listitem [ref=e339]:
                - link "Slack" [ref=e350] [cursor=pointer]:
                  - /url: https://slack.layer5.io
                - superscript [ref=e351]
              - listitem [ref=e354]:
                - link "Meet with Team Member" [ref=e365] [cursor=pointer]:
                  - /url: https://calendar.google.com/calendar/appointments/schedules/AcZssZ3pmcApaDP4xd8hvG5fy8ylxuFxD3akIRc5vpWJ60q-HemQi80SFFAVftbiIsq9pgiA2o8yvU56?gv=true
                - superscript [ref=e366]
        - generic [ref=e377]:
          - generic [ref=e378]:
            - button [ref=e379] [cursor=pointer]
            - button [ref=e383] [cursor=pointer]
          - generic [ref=e388]:
            - generic [ref=e389]: RECOGNITION
            - generic [ref=e393]:
              - paragraph [ref=e394]: Find your latest badges here as they are awarded.
              - generic [ref=e396]:
                - text: Learn more about the
                - link "recognition program" [ref=e397] [cursor=pointer]:
                  - /url: https://badges.layer5.io
                - superscript [ref=e398]
                - text: and how you can earn badges.
              - link "See All" [ref=e402] [cursor=pointer]:
                - /url: /user/user-1?tab=badges
        - generic [ref=e411]:
          - generic [ref=e412]:
            - button [ref=e413] [cursor=pointer]
            - button [ref=e417] [cursor=pointer]
          - generic [ref=e424]:
            - generic [ref=e425]: SELF-HOSTED MESHERY
            - generic [ref=e453]:
              - paragraph [ref=e454]:
                - paragraph [ref=e455]:
                  - text: Deploy a Meshery Server to the platform of your choice. Run
                  - code [ref=e456]: mesheryctl system dashboard
                  - text: to locate Meshery UI, commonly available at
                  - link "http://localhost:9081" [ref=e457] [cursor=pointer]:
                    - /url: http://localhost:9081
              - link "Install Meshery" [ref=e459] [cursor=pointer]:
                - /url: https://meshery.io#getting-started
    - navigation [ref=e468]:
      - heading [level=6] [ref=e470]:
        - text: Active Users
        - paragraph [ref=e471]: (0)
    - generic:
      - button "Feedback" [ref=e474] [cursor=pointer]
      - generic [ref=e476]:
        - generic [ref=e477]:
          - paragraph [ref=e482]: Feedback
          - generic [ref=e489] [cursor=pointer]
        - generic [ref=e494]:
          - generic [ref=e495]:
            - button [ref=e496] [cursor=pointer]:
              - paragraph [ref=e500]: Issue
            - button [ref=e501] [cursor=pointer]:
              - paragraph [ref=e509]: Suggestion
            - button [ref=e510] [cursor=pointer]:
              - paragraph [ref=e517]: Meet Request
          - textbox "I’m having an issue with..." [ref=e519]
        - generic [ref=e521]:
          - generic [ref=e522]:
            - checkbox [ref=e524] [cursor=pointer]
            - paragraph [ref=e527]: We may email you for more information or updates
          - button "Send" [disabled]
  - alert [ref=e528]
```

# Test source

```ts
  653 | // verdict and both exit paths ("Save and Close", and saving on the way out of
  654 | // the unsaved-changes modal) are gated on it - before that, a 500 dropped the
  655 | // user back into view mode with the notification the only clue their layout had
  656 | // not been written, and the unsaved edits already discarded.
  657 | //
  658 | // Nothing else covers this: the gate is an `await` on an RTK Query result, so
  659 | // `tsc`, the build and the Jest suite are all green either way.
  660 | test.describe("dashboard layout save failure", () => {
  661 |   const openDashboardInEditMode = async (
  662 |     page: import("@playwright/test").Page,
  663 |     saveStatus: number
  664 |   ) => {
  665 |     await openDashboard(page, [layoutSaveHandler(saveStatus)]);
  666 | 
  667 |     await actionButton(page, "Edit").click();
  668 | 
  669 |     // Edit mode is on when the edit-only actions have replaced the Edit button.
  670 |     await expect(actionButton(page, "Save and Close")).toBeVisible();
  671 |     await expect(actionButton(page, "Edit")).toHaveCount(0);
  672 |   };
  673 | 
  674 |   test("keeps edit mode open when Save and Close fails", async ({ page }) => {
  675 |     await openDashboardInEditMode(page, 500);
  676 | 
  677 |     await actionButton(page, "Save and Close").click();
  678 | 
  679 |     // The failure is surfaced...
  680 |     await expect(page.getByText("failed to save layout")).toBeVisible();
  681 | 
  682 |     // ...and edit mode is still open, so the unsaved layout is still editable
  683 |     // and still savable. `Edit` reappearing here is the regression.
  684 |     await expect(actionButton(page, "Save and Close")).toBeVisible();
  685 |     await expect(actionButton(page, "Cancel")).toBeVisible();
  686 |     await expect(actionButton(page, "Edit")).toHaveCount(0);
  687 |   });
  688 | 
  689 |   // The positive control: without it the test above would still pass if the
  690 |   // button had simply stopped doing anything at all.
  691 |   test("closes edit mode when Save and Close succeeds", async ({ page }) => {
  692 |     await openDashboardInEditMode(page, 200);
  693 | 
  694 |     await actionButton(page, "Save and Close").click();
  695 | 
  696 |     await expect(page.getByText("Layout saved")).toBeVisible();
  697 |     await expect(actionButton(page, "Edit")).toBeVisible();
  698 |     await expect(actionButton(page, "Save and Close")).toHaveCount(0);
  699 |   });
  700 | });
  701 | 
  702 | // Closing the widget picker must not end the edit session. While panel
  703 | // visibility and session lifetime shared one flag, the picker's X set
  704 | // `isEditMode` false - which is the flag `useUnsavedChanges` arms both of its
  705 | // guards from - while leaving the edited layout in state and on screen. The
  706 | // result was a grid still showing changes that were no longer savable and no
  707 | // longer guarded: navigating away or reloading discarded them with no prompt.
  708 | //
  709 | // Nothing static can see this. The props type-check either way, and the loss
  710 | // only appears when a real navigation crosses a guard that has been disarmed.
  711 | test.describe("dashboard widget picker close control", () => {
  712 |   test("closes the panel only, leaving edit mode and its guards armed", async ({
  713 |     page
  714 |   }) => {
  715 |     await page.setViewportSize({ width: 1600, height: 1000 });
  716 | 
  717 |     await openDashboard(page);
  718 | 
  719 |     const gridItems = page.locator(".react-grid-item");
  720 |     await expect(gridItems.first()).toBeVisible();
  721 |     const initialWidgetCount = await gridItems.count();
  722 | 
  723 |     await actionButton(page, "Edit").click();
  724 |     await expect(page.locator('[aria-label="Close widget picker"]')).toBeVisible();
  725 | 
  726 |     // Change the layout by ADDING a widget from the picker, deliberately not by
  727 |     // removing one: `removeWidget` writes the layout through `_.set`, mutating
  728 |     // the object `savedLayout` also points at, so the unsaved-changes
  729 |     // comparison sees two equal layouts and the guard never arms - a real bug,
  730 |     // but someone else's (layer5io/meshery-cloud#6111). Adding spreads into
  731 |     // fresh arrays, so it is the path that actually exercises the guard.
  732 |     await page.locator('[aria-label^="Add "][aria-label$=" widget"]').first().click();
  733 |     await expect(gridItems).toHaveCount(initialWidgetCount + 1);
  734 | 
  735 |     await page.locator('[aria-label="Close widget picker"]').click();
  736 | 
  737 |     // The panel is gone...
  738 |     await expect(page.locator('[aria-label="Close widget picker"]')).toHaveCount(0);
  739 | 
  740 |     // ...but the session is not: the edit survives on screen, and every action
  741 |     // that can still act on it is present. `Edit` reappearing here is the
  742 |     // regression - it would mean the session ended with the edit stranded.
  743 |     await expect(gridItems).toHaveCount(initialWidgetCount + 1);
  744 |     await expect(actionButton(page, "Save")).toBeVisible();
  745 |     await expect(actionButton(page, "Save and Close")).toBeVisible();
  746 |     await expect(actionButton(page, "Cancel")).toBeVisible();
  747 |     await expect(actionButton(page, "Edit")).toHaveCount(0);
  748 | 
  749 |     // And the guard is still armed. Driven through a real in-app navigation
  750 |     // (the navbar logo is a Next `Link`, so it raises `routeChangeStart`)
  751 |     // rather than by reading state: a disarmed guard lets the route through
  752 |     // and this modal never appears.
> 753 |     await page.locator('header.MuiAppBar-root a[href="/"]').first().click();
      |                                                                     ^ Error: locator.click: Test timeout of 60000ms exceeded.
  754 |     await expect(page.getByText("Unsaved dashboard layout changes")).toBeVisible();
  755 |     await expect(page).toHaveURL(/\/dashboard/);
  756 |   });
  757 | });
  758 | 
  759 | // Saving from the unsaved-changes modal must complete the navigation it
  760 | // confirms. The save is awaited, so the rest of the handler resumes from a
  761 | // network completion rather than from the click: the state update that ends the
  762 | // session renders later than the router emits `routeChangeStart`, and a guard
  763 | // that only learns it is disarmed from a render blocks its own confirmed
  764 | // navigation - the layout saves, the modal stays, the URL does not move.
  765 | //
  766 | // Nothing static can see this, and the close-control case above stops once the
  767 | // modal appears. Driven through a real in-app navigation that LEAVES the
  768 | // dashboard: the navbar logo targets `/`, which redirects an authenticated user
  769 | // straight back here, so it cannot tell a completed navigation from a blocked
  770 | // one.
  771 | test.describe("dashboard unsaved-changes save and navigate", () => {
  772 |   test("saves once and completes the navigation it confirmed", async ({ page }) => {
  773 |     await page.setViewportSize({ width: 1600, height: 1000 });
  774 | 
  775 |     const layoutSaves: string[] = [];
  776 |     page.on("request", request => {
  777 |       if (isLayoutSave(request)) {
  778 |         layoutSaves.push(request.url());
  779 |       }
  780 |     });
  781 | 
  782 |     await openDashboard(page, [layoutSaveHandler(200)]);
  783 | 
  784 |     const gridItems = page.locator(".react-grid-item");
  785 |     await expect(gridItems.first()).toBeVisible();
  786 |     const initialWidgetCount = await gridItems.count();
  787 | 
  788 |     await actionButton(page, "Edit").click();
  789 |     await expect(page.locator('[aria-label="Close widget picker"]')).toBeVisible();
  790 | 
  791 |     // ADDED rather than removed, for the reason recorded in the close-control
  792 |     // case: removal never arms the guard (layer5io/meshery-cloud#6111).
  793 |     await page.locator('[aria-label^="Add "][aria-label$=" widget"]').first().click();
  794 |     await expect(gridItems).toHaveCount(initialWidgetCount + 1);
  795 | 
  796 |     await page.getByTestId("nav-menu-toggle").click();
  797 |     await page.getByTestId("nav-item-catalog").click();
  798 | 
  799 |     const unsavedModalTitle = page.getByText("Unsaved dashboard layout changes");
  800 |     await expect(unsavedModalTitle).toBeVisible();
  801 |     await expect(page).toHaveURL(/\/dashboard/);
  802 |     expect(layoutSaves).toHaveLength(0);
  803 | 
  804 |     await page.getByRole("button", { name: "Save Changes", exact: true }).click();
  805 | 
  806 |     await expect(page).toHaveURL(/\/catalog/);
  807 |     await expect(unsavedModalTitle).toHaveCount(0);
  808 |     expect(layoutSaves).toHaveLength(1);
  809 |   });
  810 | });
  811 | 
  812 | // Closing the picker must not be a one-way door. `DashboardLayout` renders the
  813 | // picker in exactly two branches - `isSidebarOpen && isMobile` (bottom sheet
  814 | // plus Sistent's own FAB) and `isSidebarOpen && !isMobile` (the sticky panel) -
  815 | // so with the flag false NEITHER renders, at every breakpoint, and above `md`
  816 | // Sistent ships no reopen affordance at all. Without the action row's own
  817 | // "Add Widgets" entry the session survives the X but can never reach the picker
  818 | // again: the only ways out are committing a half-built layout or discarding it.
  819 | //
  820 | // Driven at desktop width, where the sticky panel branch renders and Sistent's
  821 | // mobile FAB is therefore not present to mask the gap.
  822 | test.describe("dashboard widget picker reopen", () => {
  823 |   test("reopens the picker from the action row without cycling edit mode", async ({
  824 |     page
  825 |   }) => {
  826 |     await page.setViewportSize({ width: 1600, height: 1000 });
  827 | 
  828 |     await openDashboard(page);
  829 | 
  830 |     const picker = page.locator('[aria-label="Close widget picker"]');
  831 | 
  832 |     await actionButton(page, "Edit").click();
  833 | 
  834 |     // While the panel is open the reopen action is noise, so it must not render.
  835 |     await expect(picker).toBeVisible();
  836 |     await expect(actionButton(page, "Add Widgets")).toHaveCount(0);
  837 | 
  838 |     await picker.click();
  839 | 
  840 |     // The panel is gone, the session is not - and the way back is on screen.
  841 |     await expect(picker).toHaveCount(0);
  842 |     await expect(actionButton(page, "Add Widgets")).toBeVisible();
  843 |     await expect(actionButton(page, "Save")).toBeVisible();
  844 |     await expect(actionButton(page, "Save and Close")).toBeVisible();
  845 |     await expect(actionButton(page, "Cancel")).toBeVisible();
  846 |     await expect(actionButton(page, "Edit")).toHaveCount(0);
  847 | 
  848 |     await actionButton(page, "Add Widgets").click();
  849 | 
  850 |     // Back to the open state, with the reopen action retired again.
  851 |     await expect(picker).toBeVisible();
  852 |     await expect(actionButton(page, "Add Widgets")).toHaveCount(0);
  853 |     await expect(actionButton(page, "Edit")).toHaveCount(0);
```