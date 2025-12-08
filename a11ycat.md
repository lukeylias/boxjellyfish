```
       ____ ____                      __
_____ /_   /_   |___.__. ____ _____ _/  |_
\__  \ |   ||   <   |  |/ ___\\__  \\   __\
 / __ \|   ||   |\___  \  \___ / __ \|  |
(____  /___||___|/ ____|\___  >____  /__|
     \/          \/         \/     \/
```

---

name: accessibility-audit
description: Audit React, HTML, CSS, and JavaScript code for WCAG 2.2 compliance (Levels A, AA, AAA). Use when reviewing code for accessibility issues, checking components for WCAG violations, evaluating semantic HTML, ARIA usage, keyboard navigation, color contrast, focus management, or when user requests accessibility review, a11y audit, WCAG compliance check, or screen reader compatibility assessment.

---

# Accessibility Audit

Systematic auditing of web code against WCAG 2.2 standards with focus on React components and modern web patterns.

## Audit Process

Follow this systematic approach when auditing code:

1. **Document Structure** - Semantic HTML, landmarks, heading hierarchy
2. **Keyboard Navigation** - Tab order, focus management, keyboard shortcuts
3. **Visual Elements** - Images, icons, color contrast, text sizing
4. **Interactive Components** - Forms, buttons, links, custom controls
5. **Dynamic Content** - ARIA live regions, state changes, client-side routing
6. **React-Specific** - Fragments, portals, hooks effects on a11y

## Critical WCAG 2.2 Success Criteria

### Level A (Must Have)

**1.1.1 Non-text Content**

- All images need alt text: `<img src="..." alt="description">`
- Decorative images: `<img src="..." alt="">` or `aria-hidden="true"`
- Icon buttons: `<button aria-label="Close"><Icon /></button>`
- SVGs: Include `<title>` or `aria-label`

**1.3.1 Info and Relationships**

- Use semantic HTML: `<nav>`, `<main>`, `<header>`, `<footer>`, `<article>`, `<aside>`
- Proper heading hierarchy: h1 → h2 → h3 (no skipping)
- Lists use `<ul>`, `<ol>`, `<dl>` elements
- Form labels: `<label htmlFor="email">Email</label><input id="email" />`

**2.1.1 Keyboard**

- All interactive elements keyboard accessible
- No keyboard traps (user can navigate away)
- Avoid `onClick` on non-interactive elements (use buttons instead)
- Custom components: `tabIndex={0}` and `onKeyDown` handlers

**2.4.1 Bypass Blocks**

- Skip navigation link: `<a href="#main">Skip to content</a>`
- Proper landmark roles for screen readers

**4.1.2 Name, Role, Value**

- Native HTML elements preferred (inherent roles)
- Custom components need ARIA: `role="button"`, `aria-pressed`, `aria-expanded`

### Level AA (Should Have)

**1.4.3 Contrast (Minimum)**

- Text contrast: 4.5:1 for normal text, 3:1 for large text (18pt+ or 14pt+ bold)
- Check: body text, links, button text, placeholder text
- Tools: Browser DevTools color picker shows ratios

**1.4.5 Images of Text**

- Avoid text in images; use CSS/HTML text instead
- Exception: logos, essential images

**2.4.6 Headings and Labels**

- Descriptive headings: "Contact Information" not "Info"
- Clear form labels: "Email Address" not "Email"

**2.4.7 Focus Visible**

- Visible focus indicator on all interactive elements
- Don't remove outline without custom focus styles
- React: manage focus in modals, route changes

**3.2.3 Consistent Navigation**

- Navigation in same order across pages
- React: consistent header/footer components

**3.3.1 Error Identification**

- Form errors clearly described
- React: `aria-invalid="true"` and `aria-describedby="error-id"`

**3.3.2 Labels or Instructions**

- All form inputs have labels
- Required fields marked: `<input required aria-required="true" />`

### Level AAA (Nice to Have)

**2.4.9 Link Purpose (Link Only)**

- Links descriptive without context: "Read our accessibility policy" not "Click here"

**2.4.10 Section Headings**

- Content organized with headings

## React-Specific Patterns

### Focus Management

```jsx
// After modal opens
useEffect(() => {
  if (isOpen) {
    modalRef.current?.focus();
  }
}, [isOpen]);

// Trap focus in modal
const handleKeyDown = (e) => {
  if (e.key === "Escape") closeModal();
  // Implement focus trap logic
};
```

### Announcing Dynamic Changes

```jsx
// Live region for announcements

{
  message;
}

// For urgent alerts

{
  error;
}
```

### Accessible Custom Components

```jsx
// Custom button
<div
  role="button"
  tabIndex={0}
  onClick={handleClick}
  onKeyDown={(e) => {
    if (e.key === 'Enter' || e.key === ' ') {
      e.preventDefault();
      handleClick();
    }
  }}
  aria-pressed={isPressed}
>
  {children}


// Better: use native button

  {children}

```

### Client-Side Routing

```jsx
// Announce route changes
useEffect(() => {
  document.title = `${pageTitle} - Site Name`;
  // Move focus to main content
  mainRef.current?.focus();
}, [location]);
```

## Common Anti-Patterns

**❌ Avoid:**

- `<div onClick={...}>` - Use `<button>` instead
- Missing alt text on images
- `tabIndex={-1}` on interactive elements (removes from tab order)
- Color alone to convey information
- Removing focus outlines without replacement: `outline: none`
- `<div>` or `<span>` as buttons without proper ARIA
- Placeholder text as labels
- Auto-playing audio/video without controls

**✅ Do:**

- Use semantic HTML elements
- Provide text alternatives for non-text content
- Ensure keyboard operability
- Give users enough time to read/use content
- Design for screen readers
- Make forms accessible with labels and errors
- Test with keyboard only, screen readers

## Reporting Format

Report violations using this structure:

```
**Severity:** [Critical/High/Medium/Low]
**WCAG:** [Criterion number and level]
**Location:** [File and line number]
**Issue:** [Clear description of problem]
**Impact:** [Who is affected and how]
**Fix:** [Specific actionable solution]
**Code:** [Show problematic code and corrected version]
```

### Severity Guidelines

- **Critical (Level A):** Blocks access for users with disabilities
- **High (Level AA):** Significant barrier, required for compliance
- **Medium (Level AA):** Usability issue, recommended fix
- **Low (Level AAA):** Enhancement, improves experience

## Testing Checklist

- [ ] Keyboard-only navigation (no mouse)
- [ ] Screen reader testing (NVDA, JAWS, VoiceOver)
- [ ] Color contrast verification
- [ ] Zoom to 200% - content reflows
- [ ] Browser DevTools Lighthouse accessibility audit
- [ ] Check focus order and visibility
- [ ] Test with browser extensions (axe DevTools)
- [ ] Verify ARIA usage is correct and necessary

## Quick Reference

**Semantic HTML:** main, nav, header, footer, article, section, aside, button, a
**ARIA Roles:** button, link, navigation, banner, contentinfo, complementary, search
**ARIA States:** aria-expanded, aria-pressed, aria-checked, aria-selected, aria-current
**ARIA Properties:** aria-label, aria-labelledby, aria-describedby, aria-hidden
**Live Regions:** aria-live (polite/assertive), role="status", role="alert"

## Resources

For detailed WCAG 2.2 criteria: https://www.w3.org/WAI/WCAG22/quickref/
For ARIA patterns: https://www.w3.org/WAI/ARIA/apg/patterns/
