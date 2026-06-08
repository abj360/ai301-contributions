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

1. Open the SvelteKit demo project in a narrow browser window so the page is scrollable
2. Add a navigation link on the About page pointing to another route
3. Scroll to the bottom of the About page
4. Click the navigation link
5. **Observed result:** the destination page loads but the window is still scrolled to the same Y position

### Reproduction Evidence

- **Commit showing reproduction:** [Link to commit in your fork]
- **Screenshots/logs:** [If applicable]
- **My findings:** [What you discovered during reproduction]

---

## Solution Approach

### Analysis

[Your analysis of the root cause — what's causing the issue?]

### Proposed Solution

[High-level description of your fix approach]

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** On client-side navigation, SvelteKit should call `window.scrollTo(0, 0)` (or equivalent) after the new page is rendered. The regression suggests this call was removed or is now gated behind a condition that evaluates to false.

**Match:** [What similar patterns/solutions exist in the codebase?]

**Plan:**
1. Diff `next-192` and `next-193` to find the scroll-related change
2. Locate where the router triggers scroll reset in `packages/kit/src/runtime/client/`
3. Restore or fix the scroll-to-top call so it fires on every non-hash navigation

**Implement:** [Link to your branch/commits as you work]

**Review:** [Self-review checklist — does it follow the project's contribution guidelines?]

**Evaluate:** Manually navigate between pages in the demo app from a scrolled-down state and verify the scroll resets to the top each time.

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
