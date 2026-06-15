# Contribution #1: Page scroll position not reset to top on navigation (regression)

**Contribution Number:** 1  
**Student:** Toyosi Abolaji  
**Issue:** https://github.com/sveltejs/kit/issues/2733  
**Status:** Phase I — In Progress

---

## Why I Chose This Issue

This issue caught my attention because it describes a regression in core navigation behavior — something every SvelteKit app depends on. Scroll management seems simple on the surface, but it sits at the intersection of the router, the browser's native behavior, and SvelteKit's client-side navigation lifecycle, which makes it a great way to learn how the framework handles page transitions under the hood.

I also chose it because the reproduction steps are clearly documented and the regression was pinpointed to a specific version (`next-192` → `next-193`), which means I can do a focused git bisect-style investigation rather than hunting blindly. The "help wanted" label signals that the maintainers are open to outside contributions, which increases the chance my work will get reviewed and merged.

---

## Understanding the Issue

### Problem Description

When a user navigates to a new page in SvelteKit while the current page is scrolled down, the scroll position is not reset to the top of the new page. The browser keeps whatever Y offset the previous page had, so the destination page loads in a scrolled-down state. This is a regression — it worked correctly before version `next-193`.

### Expected Behavior

On every client-side page navigation, the scroll position should reset to the top of the page (Y = 0), matching standard full-page browser navigation behavior.

### Current Behavior

After clicking a navigation link from a scrolled-down position, the new page renders but the window remains scrolled to the same Y offset as the previous page, giving the user no visual indication that a new page has loaded.

### Affected Components

- SvelteKit's client-side router (navigation / history handling)
- Scroll management / restoration logic (likely in `packages/kit/src/runtime/client/`)

---

## Reproduction Process

### Environment Setup

[Notes on setting up your local development environment — challenges you faced, how you solved them]

### Steps to Reproduce

1. Clone the SvelteKit repository and check out the `next` branch
2. Run `npm install` to install dependencies
3. Run `npm run dev` to start the development server
4. Create a test page with content tall enough to require scrolling (e.g., 200+ viewport heights)
5. Scroll the page down to approximately 500px or more
6. Click a navigation link to a different route (e.g., from `/` to `/about`)
7. **Observed result:** The new page loads but `window.scrollY` remains at the previous scroll position instead of resetting to 0
8. **Expected result:** The new page should automatically scroll to the top (Y = 0)

### Reproduction Evidence

- **Fork branch:** https://github.com/abj360/kit
- **Commit showing reproduction:** [Visible in fork commits on main branch]
- **My findings:** The regression was introduced when scroll restoration logic was refactored. On client-side navigation, the scroll position is not being reset to the top, causing navigated pages to display at the scroll offset of the previous page.

---

## Solution Approach

### Analysis

[Your analysis of the root cause — what's causing the issue?]

### Proposed Solution

[High-level description of your fix approach]

### Implementation Plan

**Approach:**
1. Examine the client-side router in `packages/kit/src/runtime/client/router.js` to locate where navigation state is managed
2. Identify the scroll position management logic that should fire after page transition
3. Add or restore a `window.scrollTo(0, 0)` call that executes when navigating to a new route (excluding hash-only changes)
4. Ensure the scroll reset happens after the new page DOM is committed, not before
5. Add test coverage for scroll-to-top behavior on client-side navigation
6. Verify hash navigation and back/forward behavior are not affected by the fix

**Validation:**
- Manual navigation from scrolled state should result in destination page at Y=0
- Hash navigation (e.g., `#section`) should preserve scroll behavior
- Browser back/forward should restore previous scroll positions
- Tests should pass without regression

---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: Navigating to a new route resets scroll to (0, 0)
- [ ] Test case 2: Navigating to an anchor hash preserves / sets the correct scroll position
- [ ] Test case 3: Back/forward browser navigation restores the scroll position of the target page

### Integration Tests

- [ ] Full navigation flow: scroll down → navigate → verify scroll at top
- [ ] Hash navigation is not broken by the fix

### Manual Testing

[What you tested manually and results]

---

## Implementation Notes

### Week 1 Progress

[What you built this week, challenges faced, decisions made]

### Code Changes

- **Files modified:** [List]
- **Key commits:** [Links to important commits]
- **Approach decisions:** [Why you chose certain approaches]

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** [Draft or final PR description]

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [SvelteKit routing docs](https://kit.svelte.dev/docs/routing)
- [GitHub issue #2733](https://github.com/sveltejs/kit/issues/2733)
- [SvelteKit contribution guide](https://github.com/sveltejs/kit/blob/main/CONTRIBUTING.md)
