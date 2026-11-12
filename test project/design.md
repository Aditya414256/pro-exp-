# MediFind — UI/UX Design System

**Design status:** Baseline extracted from current frontend files and established project direction. This is a consistency guide, not permission to redesign the application.

## 1. Brand and visual direction

MediFind should feel like a modern, calm, trustworthy healthcare utility. The visual language is minimal, clean and practical. Prioritize clarity, readability, predictable navigation and confidence in pharmacy information over decoration.

- Product name: **MediFind**
- Primary direction: minimal healthcare / modern web application
- General page background: light neutral
- Main content: white surfaces on a subtle neutral canvas
- Primary action: blue
- Positive/verified state: green
- Avoid excessive gradients, shadows, colored panels and decorative elements.

## 2. Typography

The current frontend imports Google Fonts:

- **Plus Jakarta Sans** — body copy, controls, labels, navigation and general UI.
- **Space Grotesk** — brand wordmark and prominent headings.

Use a clear hierarchy:
- Page title: Space Grotesk, bold.
- Section title: Space Grotesk or the existing page heading style.
- Body and supporting text: Plus Jakarta Sans.
- Buttons and navigation: Plus Jakarta Sans, medium/semibold.
- Small metadata: Plus Jakarta Sans, muted color, readable size.

Do not introduce a third font without a concrete need.

## 3. Color tokens

Use the following established project palette consistently. Verify the exact existing page styles before changing values.

| Token | Value | Use |
|---|---|---|
| Primary | `#0284c7` | Primary buttons, links, focus |
| Primary dark | `#0369a1` | Hover and emphasized blue |
| Primary soft | `#e0f2fe` | Subtle blue accents |
| Accent / success | `#059669` | Positive accent |
| Verified green | `#047857` | Verified state text/icon |
| Verified background | `#d1fae5` | Verified badge background |
| Page background | `#f8fafc` | Main page canvas |
| Surface | `#ffffff` | Cards, inputs, navigation |
| Border | `#e2e8f0` | Dividers, card/input borders |
| Main text | `#0f172a` | Headings and primary content |
| Muted text | `#64748b` | Supporting copy |
| Placeholder | `#94a3b8` | Input placeholder |

Primary gradient, only where already used or explicitly approved:

`linear-gradient(135deg, #0284c7 0%, #0369a1 100%)`

Do not use color alone to communicate status; include text or an accessible label.

## 4. Layout and spacing

- Use centered content containers with a sensible maximum width; the Find Medicine and Pharmacies pages currently use approximately 1100px.
- Keep page padding comfortable and consistent.
- Use a spacing scale based on 4px increments.
- Maintain clear vertical rhythm between page title, supporting text, forms and results.
- Keep forms and result cards aligned.
- Use responsive layouts; avoid fixed widths that cause horizontal overflow.
- Preserve the current page-specific layout unless the requested task requires a change.

## 5. Navigation

The current shared navigation uses a three-section grid:
1. Brand/logo at the left.
2. Main navigation centered.
3. Login/Register actions at the right.

Current visible navigation in the inspected Home and Find Medicine HTML includes Home and Pharmacies. Session-aware behaviour is handled by the navbar script. Do not add removed navigation items or change navigation structure without approval.

- Navbar height in current CSS: 72px.
- White background with a subtle bottom border.
- Brand uses a blue plus icon and MediFind wordmark.
- Navigation is visually quiet; hover uses the primary blue.
- Login is a neutral outlined button.
- Register is a filled primary action.

## 6. Components

### Buttons
- Primary: blue fill, white text, medium/semibold weight.
- Secondary: white or transparent surface with neutral border.
- Destructive: reserved for destructive actions and clearly labelled.
- Provide hover, focus, active and disabled states.
- Keep button text action-oriented.

### Inputs
- White background.
- Neutral border, rounded corners.
- Clear focus treatment using primary blue.
- Readable placeholder and label.
- Show validation feedback adjacent to the relevant field.

### Cards
- White surface.
- Neutral border.
- Rounded corners (existing pages commonly use 16px).
- Subtle shadow only where already part of the design.
- Consistent internal spacing.
- Keep information hierarchy clear.

### Status badges
- Use restrained semantic colors for verified, pending, rejected, available, low-stock and out-of-stock states.
- Always show readable status text.
- Do not use a green badge for every neutral label.

### Empty, loading and error states
- Explain what is happening or what the user can do next.
- Avoid fake data and unexplained blank areas.
- Use concise messages and preserve the page layout.

## 7. Page-specific direction

### Home page
- Keep the brand/navigation clean.
- Main message should communicate medicine discovery and verified pharmacies.
- Search medicine or generic name is the primary action.
- Autocomplete suggestions should be easy to scan and keyboard accessible.
- Do not add a colored background panel to the intro/header section without approval.

### Find Medicine page
- Keep the search input prominent.
- Display medicine details and available pharmacy/order choices clearly.
- **Do not add the removed filter sidebar or filter section.**
- Preserve the existing selection flow and its API integration.
- Avoid unrelated redesign of order dialogs or cards.

### Pharmacies page
- Prioritize pharmacy name, location, verification status and supported services.
- Make verified status understandable without implying medical endorsement.
- Keep cards consistent with the rest of the application.

### Login and registration
- Keep forms focused, readable and accessible.
- Clearly distinguish customer and pharmacy registration.
- Provide inline validation and clear error states.
- Avoid unnecessary decorative sections.

### Pharmacy and admin dashboards
- Prioritize operational information: orders, inventory, prescription review, requests and verification.
- Use clear status labels and safe confirmation for consequential actions.
- Preserve role-specific navigation and access restrictions.

## 8. Responsive behaviour

- Desktop: preserve current centered content and three-part navigation where it fits.
- Tablet: reduce gaps and allow navigation/actions to wrap or collapse intentionally.
- Mobile: stack content, make controls full-width where appropriate, keep tap targets comfortable and prevent horizontal scrolling.
- Cards should collapse from multi-column to single-column layouts as needed.
- Test at narrow viewport widths after CSS changes.

## 9. Accessibility requirements

- Use semantic headings in logical order.
- Associate visible labels with form controls.
- Ensure keyboard access and visible focus.
- Use sufficient text/background contrast.
- Provide accessible names for icon-only controls.
- Announce dynamic validation and status feedback appropriately.
- Do not encode meaning only through color.

## 10. Design change checklist

Before finalizing a UI change, verify:
- Does it match the existing MediFind visual language?
- Did it preserve the user's requested page structure?
- Did it avoid reintroducing removed sections?
- Does it work on desktop and mobile?
- Are all states (hover, focus, disabled, loading, error, empty) considered?
- Is the UI accessible and consistent with neighbouring pages?
- Were unrelated styles left untouched?
