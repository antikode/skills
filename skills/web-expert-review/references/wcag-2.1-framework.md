# WCAG 2.1 Accessibility Framework

Reference guide for evaluating website accessibility compliance.

---

## Overview

Web Content Accessibility Guidelines (WCAG) 2.1 is the international standard for web accessibility. It provides testable criteria across three conformance levels: **A** (minimum), **AA** (recommended), **AAA** (enhanced).

For most commercial Indonesian enterprise websites, audit to **WCAG 2.1 AA** level.

---

## Four Principles (POUR)

### 1. **Perceivable**
Information and user interface components must be presentable in ways users can perceive.

#### Key Criteria to Check:
- **Color Contrast** (Level AA): Text must have a contrast ratio of at least 4.5:1 for normal text, 3:1 for large text
- **Text Alternatives** (Level A): All images have descriptive alt text; decorative images marked as such
- **Captions & Transcripts** (Level A): Video content has captions; audio has transcripts
- **Adjustable Text** (Level AA): Users can resize text up to 200% without loss of functionality
- **Visual Focus Indicators** (Level AA): Interactive elements have visible focus states

#### Common Issues on Indonesian Sites:
- Insufficient contrast on white/light-colored text on light backgrounds
- Missing alt text on product/promotional images
- Embedded videos without captions or transcripts
- No text resize capability in custom font systems

---

### 2. **Operable**
User interface components and navigation must be operable via keyboard and other input methods.

#### Key Criteria to Check:
- **Keyboard Navigation** (Level A): All functionality accessible via keyboard; tab order logical
- **Keyboard Traps** (Level A): Users can navigate away from any component using keyboard alone
- **Focus Visible** (Level AA): Keyboard focus indicator is always visible
- **Target Size** (Level AAA): Touch targets are at least 44x44 CSS pixels
- **Timing Controls** (Level A): Users can pause, stop, or adjust timing of auto-playing content

#### Common Issues on Indonesian Sites:
- Mega menus that trap keyboard focus
- Floating CTAs that block content without dismiss option
- WhatsApp chat widgets with no keyboard escape
- Hamburger menus on mobile without visible focus states
- Auto-playing video/carousel on homepage without pause control

---

### 3. **Understandable**
Information and user interface operation must be understandable.

#### Key Criteria to Check:
- **Language Declaration** (Level A): Page language declared in HTML (`<html lang="id">`)
- **Readable Text** (Level A): Page uses clear language; uncommon words are defined or abbreviated
- **Consistent Navigation** (Level AA): Navigation menus appear in consistent location across pages
- **Consistent Functionality** (Level AA): Interactive components behave consistently (e.g., buttons always look the same)
- **Error Identification** (Level A): Form errors are clearly identified and described
- **Error Prevention** (Level AA): Form submissions are reversible or confirmed before commit

#### Common Issues on Indonesian Sites:
- Mixed language content (Indonesian/English) without clear separation
- Form errors described only in red color (not accessible to colorblind users)
- Contact forms with no confirmation before submission
- No "success" message after form submission
- Inconsistent button styling across pages

---

### 4. **Robust**
Code must be robust enough to be reliably interpreted by assistive technologies (screen readers, etc.).

#### Key Criteria to Check:
- **Valid HTML** (Level A): No duplicate IDs; properly nested elements
- **ARIA Labels** (Level A): Interactive components have accessible names (via `aria-label`, `aria-labelledby`, or visible text)
- **Semantic HTML** (Level A): Use `<button>`, `<nav>`, `<main>`, `<article>` instead of `<div>` wrappers
- **Form Labels** (Level A): All form inputs have associated `<label>` elements
- **Name, Role, Value** (Level A): All UI components have accessible name, role, and state/value

#### Common Issues on Indonesian Sites:
- Icon buttons with no aria-label (screen reader users don't know what they do)
- `<div onclick>` instead of `<button>` for interactive elements
- Form fields with no associated labels (only placeholder text)
- Unstructured navigation (lots of `<div>` instead of `<nav>`)
- Modals/overlays without proper focus management

---

## Conformance Levels

| Level | Standard | Recommendation | Timeline |
|-------|----------|-----------------|----------|
| **A** | Minimum | Foundation; comply before launch | Immediate |
| **AA** | Recommended | Industry standard for public websites | 3–6 months |
| **AAA** | Enhanced | Go-beyond (news sites, government) | 6–12 months |

---

## Quick Audit Checklist (WCAG 2.1 AA)

### Visual Design
- [ ] Text contrast is 4.5:1 for body text, 3:1 for large text
- [ ] Color is not the only way to convey information (e.g., errors shown in red + icon)
- [ ] Interactive elements have visible focus indicators
- [ ] Touch targets are at least 44x44 pixels

### Images & Media
- [ ] All meaningful images have descriptive alt text
- [ ] Decorative images have empty alt text (`alt=""`)
- [ ] Videos have captions and transcripts
- [ ] Images with text are not the only way to convey information

### Navigation & Structure
- [ ] Page has a clear heading hierarchy (`<h1>`, `<h2>`, `<h3>`)
- [ ] Navigation is consistent across pages
- [ ] All page content is reachable via keyboard
- [ ] Links have descriptive text (not "Click here")

### Forms
- [ ] All form fields have associated labels
- [ ] Form errors are clearly identified and described
- [ ] Users can recover from submission errors
- [ ] Error messages don't rely on color alone

### Code & Semantics
- [ ] HTML is valid (no duplicate IDs)
- [ ] Semantic HTML is used (`<button>`, `<nav>`, `<main>`, etc.)
- [ ] ARIA is used correctly where semantic HTML isn't possible
- [ ] Focus order is logical and matches visual order

---

## Tools for Testing

- **WAVE** (WebAIM) — Browser extension for visual feedback
- **axe DevTools** — Automated scanning + manual testing
- **Lighthouse** (Chrome DevTools) — Built-in accessibility audit
- **NVDA** (Windows) / **JAWS** (Windows) / **VoiceOver** (macOS) — Screen reader testing
- **Color Contrast Analyzer** — Check specific color pairs

---

## Indonesian Market Considerations

- **Bilingual Content:** If site is ID + English, declare language per section (`<section lang="en">`) and ensure both versions meet WCAG AA
- **Mobile-First:** Indonesian users primarily access via mobile; ensure touch targets are thumb-friendly
- **Age Demographics:** Older demographics (40+) may have vision issues; contrast and readability are critical
- **Assistive Tech Adoption:** Screen reader usage lower than Western markets; keyboard navigation is the safety net

---

## References

- [WCAG 2.1 Official Specification](https://www.w3.org/WAI/WCAG21/quickref/)
- [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)
- [Accessible Rich Internet Applications (ARIA) Authoring Practices](https://www.w3.org/WAI/ARIA/apg/)

---

**Last Updated:** March 2026  
**Applicable To:** Antikode web-expert-review skill audits
