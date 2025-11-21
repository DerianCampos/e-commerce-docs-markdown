# UI/UX Guidelines - Electronics Store for Programmers

Comprehensive design system, UI/UX principles, and component specifications for the Electronics Store for Programmers platform.

---

## Design Philosophy

### Core Principles

1. **Developer-First**: Design for technical users who value efficiency and information density
2. **Clarity Over Decoration**: Clean, functional design that prioritizes usability
3. **Performance**: Fast, responsive interfaces that don't sacrifice aesthetics
4. **Accessibility**: WCAG 2.1 AA compliant, keyboard navigable, screen reader friendly
5. **Consistency**: Predictable patterns and interactions across the platform

---

## Design System

### Color Palette

#### Primary Colors

- **Primary**: `#1890ff` (Ant Design Blue) - CTAs, links, active states
- **Primary Hover**: `#40a9ff`
- **Primary Active**: `#096dd9`
- **Primary Light**: `#e6f7ff`

#### Secondary Colors

- **Success**: `#52c41a` - Success messages, positive actions
- **Warning**: `#faad14` - Warnings, cautions
- **Error**: `#ff4d4f` - Errors, destructive actions
- **Info**: `#1890ff` - Informational messages

#### Neutral Colors

- **Text Primary**: `#000000d9` (rgba(0,0,0,0.85))
- **Text Secondary**: `#00000073` (rgba(0,0,0,0.45))
- **Text Disabled**: `#00000040` (rgba(0,0,0,0.25))
- **Border**: `#d9d9d9`
- **Background**: `#ffffff`
- **Background Secondary**: `#fafafa`

#### Dark Mode Colors

- **Background**: `#141414`
- **Background Secondary**: `#1f1f1f`
- **Text Primary**: `#ffffffd9` (rgba(255,255,255,0.85))
- **Text Secondary**: `#ffffff73` (rgba(255,255,255,0.45))
- **Border**: `#434343`

### Typography

#### Font Families

- **Primary**: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif
- **Monospace**: 'SF Mono', Monaco, 'Cascadia Code', 'Roboto Mono', Consolas, monospace

#### Type Scale

- **H1**: 38px, 700 weight, 1.2 line-height
- **H2**: 30px, 700 weight, 1.3 line-height
- **H3**: 24px, 600 weight, 1.4 line-height
- **H4**: 20px, 600 weight, 1.5 line-height
- **H5**: 16px, 600 weight, 1.5 line-height
- **Body Large**: 16px, 400 weight, 1.6 line-height
- **Body**: 14px, 400 weight, 1.6 line-height
- **Body Small**: 12px, 400 weight, 1.5 line-height
- **Caption**: 12px, 400 weight, 1.4 line-height

### Spacing System

Based on 8px grid:

- **XS**: 4px
- **S**: 8px
- **M**: 16px
- **L**: 24px
- **XL**: 32px
- **XXL**: 48px

### Border Radius

- **Small**: 2px (buttons, inputs)
- **Medium**: 4px (cards, modals)
- **Large**: 8px (containers)
- **Round**: 50% (avatars, badges)

### Shadows

- **Small**: `0 2px 4px rgba(0,0,0,0.08)`
- **Medium**: `0 4px 12px rgba(0,0,0,0.12)`
- **Large**: `0 8px 24px rgba(0,0,0,0.15)`

---

## Component Specifications

### Buttons

#### Primary Button

- Background: Primary color
- Text: White
- Padding: 8px 16px
- Border radius: 2px
- Font size: 14px
- Min height: 32px
- Hover: Lighter primary
- Active: Darker primary
- Disabled: Gray background, reduced opacity

#### Secondary Button

- Background: Transparent
- Border: 1px solid primary
- Text: Primary color
- Other specs same as primary

#### Ghost Button

- Background: Transparent
- Text: Primary color
- No border
- Hover: Light primary background

#### Icon Button

- Size: 32x32px
- Circular or square
- Icon centered
- Hover: Background color change

### Forms

#### Input Fields

- Height: 32px
- Padding: 4px 11px
- Border: 1px solid #d9d9d9
- Border radius: 2px
- Focus: Primary border color, box shadow
- Error: Red border, error message below

#### Labels

- Font size: 14px
- Weight: 400
- Color: Text primary
- Position: Above input, 4px margin

#### Error Messages

- Color: Error red
- Font size: 12px
- Display below input
- Icon: Error icon (optional)

### Cards

#### Product Card

- Border: 1px solid #f0f0f0
- Border radius: 4px
- Padding: 16px
- Hover: Shadow elevation, slight scale (1.02)
- Image: 16:9 aspect ratio
- Title: 16px, 600 weight, 2 lines max (ellipsis)
- Price: 18px, primary color, 700 weight
- Rating: Stars + count

#### Content Card

- Same as product card
- Flexible content area
- Optional header, footer

### Navigation

#### Header

- Height: 64px
- Background: White
- Box shadow: Small
- Sticky on scroll
- Logo: Left aligned
- Navigation: Center
- User actions: Right aligned

#### Main Navigation

- Horizontal menu
- Font size: 14px
- Active: Primary color, bottom border
- Hover: Primary color

#### Mobile Navigation

- Hamburger menu icon
- Slide-in drawer from left
- Full height
- Overlay backdrop

### Modals

#### Standard Modal

- Max width: 520px
- Padding: 24px
- Border radius: 4px
- Backdrop: rgba(0,0,0,0.45)
- Header: Title + close button
- Footer: Action buttons (right aligned)

### Tables

#### Data Table

- Bordered: Optional
- Striped rows: Optional
- Hover: Background color change
- Sortable columns: Arrow indicator
- Pagination: Bottom, right aligned
- Row actions: Right aligned icons

---

## Layout Guidelines

### Grid System

- 12-column grid
- Gutter: 16px
- Breakpoints:
  - XS: < 576px (mobile)
  - SM: ≥ 576px (mobile landscape)
  - MD: ≥ 768px (tablet)
  - LG: ≥ 992px (desktop)
  - XL: ≥ 1200px (large desktop)
  - XXL: ≥ 1600px (extra large)

### Container Widths

- XS: 100%
- SM: 540px
- MD: 720px
- LG: 960px
- XL: 1140px
- XXL: 1320px

### Page Layout

```
┌─────────────────────────────┐
│        Header (64px)        │
├─────────────────────────────┤
│                             │
│        Main Content         │
│      (min-height: calc      │
│     100vh - 64px - 80px)    │
│                             │
├─────────────────────────────┤
│        Footer (80px)        │
└─────────────────────────────┘
```

---

## Responsive Design

### Mobile-First Approach

1. Design for mobile (320px min width)
2. Enhance for tablets (768px+)
3. Optimize for desktop (992px+)

### Mobile Optimizations

- Touch targets: Minimum 44x44px
- Font sizes: Slightly larger (16px min for body)
- Navigation: Hamburger menu, drawer
- Forms: Full width inputs, larger tap targets
- Cards: Stack vertically
- Tables: Horizontal scroll or card view

### Tablet Optimizations

- 2-column product grid
- Expanded navigation (some items visible)
- Sidebar for filters (optional drawer)

### Desktop Optimizations

- 3-4 column product grid
- Full navigation visible
- Persistent filter sidebar
- Hover interactions
- Multi-column layouts

---

## Accessibility Standards

### WCAG 2.1 AA Compliance

#### Perceivable

- **Text Alternatives**: Alt text for all images
- **Color Contrast**: 4.5:1 for normal text, 3:1 for large text
- **Adaptable**: Semantic HTML, logical reading order
- **Distinguishable**: Don't rely solely on color to convey information

#### Operable

- **Keyboard Accessible**: All functionality via keyboard
- **Enough Time**: No time limits on interactions
- **Seizures**: No flashing content (< 3 flashes per second)
- **Navigable**: Skip links, clear focus indicators, descriptive headings

#### Understandable

- **Readable**: Language attribute, clear labels
- **Predictable**: Consistent navigation, clear errors
- **Input Assistance**: Clear labels, error identification, help text

#### Robust

- **Compatible**: Valid HTML, ARIA where appropriate
- **Screen Reader Support**: ARIA labels, roles, live regions

### Focus Indicators

- Visible focus outline: 2px solid primary color
- Offset: 2px from element
- Never remove focus styles

### ARIA Usage

- Use semantic HTML first
- Add ARIA attributes when needed:
  - `aria-label` for icon buttons
  - `aria-describedby` for form help text
  - `aria-live` for dynamic content
  - `role` for custom components

---

## Interaction Patterns

### Loading States

- **Skeleton Screens**: For initial page loads
- **Spinners**: For button actions
- **Progress Bars**: For multi-step processes
- **Shimmer Effect**: For content loading

### Empty States

- Illustration or icon
- Descriptive message
- Call-to-action button (if applicable)
- Example: "No products found. Try adjusting your filters."

### Error States

- Error icon (red)
- Clear error message
- Suggested action to resolve
- Retry button (if applicable)

### Success States

- Success icon (green)
- Confirmation message
- Next step guidance
- Auto-dismiss after 3-5 seconds (for toasts)

---

## Animation Guidelines

### Transitions

- Duration: 200-300ms for most interactions
- Easing: ease-in-out for smooth motion
- Properties: opacity, transform (not width/height for performance)

### Microinteractions

- Button hover: Slight color change
- Card hover: Shadow elevation, subtle scale
- Input focus: Border color change, box shadow
- Loading: Spinner or pulse animation

### Page Transitions

- Fade in: New page content
- Slide in: Modals, drawers
- Scale: Image zoom, modal open/close

---

## Iconography

### Icon System

- **Library**: Ant Design Icons
- **Size**: 16px (default), 20px (large), 24px (extra large)
- **Color**: Inherit from parent or semantic colors
- **Style**: Outlined (default), filled (for active states)

### Common Icons

- **Navigation**: Menu, Close, ChevronRight, ChevronDown
- **Actions**: Edit, Delete, Add, Search, Filter
- **Status**: Check, Warning, Error, Info
- **E-commerce**: Cart, Heart, Star, User, CreditCard

---

## Content Guidelines

### Writing Style

- **Tone**: Professional but friendly
- **Voice**: Active, direct, second person ("You can...")
- **Length**: Concise, scannable
- **Technical Accuracy**: Correct specs, no jargon without explanation

### Microcopy

- **Buttons**: Action-oriented ("Add to Cart", not "Submit")
- **Errors**: Specific and helpful ("Email is already registered" not "Error")
- **Empty States**: Encouraging ("Start by adding products")
- **Success**: Positive reinforcement ("Order placed successfully!")

---

## Design Tools

### Figma Setup

- **File Structure**: Pages for Design System, Wireframes, High-Fidelity Designs
- **Components**: Buttons, Inputs, Cards, Navigation, Modals
- **Styles**: Colors, Typography, Shadows, Grid
- **Plugins**: Ant Design, Iconify, Auto Layout

### Prototyping

- Interactive prototypes for user flows
- Click-through for key journeys
- Animations for microinteractions
- Mobile and desktop versions

---

## Handoff Process

### Design to Development

1. **Finalize Designs**: Ensure all states designed (default, hover, active, disabled, error)
2. **Specifications**: Provide spacing, colors, typography for each component
3. **Assets**: Export icons, images, illustrations in appropriate formats
4. **Prototype**: Share interactive prototype for reference
5. **Documentation**: Write component notes in Figma
6. **Review**: Walk through designs with developers
7. **Support**: Available during implementation for questions

### Component Checklist

- [ ] Default state designed
- [ ] Hover state designed
- [ ] Active/focus state designed
- [ ] Disabled state designed
- [ ] Error state designed (if applicable)
- [ ] Loading state designed (if applicable)
- [ ] Responsive behavior defined
- [ ] Accessibility notes provided
- [ ] Specifications documented
- [ ] Assets exported

---

## Testing & Validation

### Usability Testing

- **MVP**: Beta testing with 20-50 users
- **Ongoing**: Monthly usability tests (5-8 users)
- **Methods**: Task-based scenarios, think-aloud protocol
- **Focus Areas**: Registration, product discovery, checkout

### A/B Testing

- **Test Ideas**: CTA button text, product card layout, filter UI
- **Tools**: Google Optimize, custom implementation
- **Metrics**: Conversion rate, click-through rate, time on task

### Accessibility Testing

- **Tools**: axe DevTools, WAVE, Lighthouse
- **Screen Readers**: NVDA (Windows), JAWS (Windows), VoiceOver (Mac/iOS)
- **Keyboard Navigation**: Tab through all interactive elements
- **Color Contrast**: Check all text and interactive elements

---

## Resources

### Design Assets

- Figma Design System: [Link to Figma file]
- Icon Library: Ant Design Icons
- Stock Photos: Unsplash, Pexels (product placeholders)

### Inspiration

- Dribbble (e-commerce design)
- Behance (UI/UX case studies)
- Awwwards (web design excellence)

### Tools

- Figma (design and prototyping)
- FigJam (user flows, wireframes)
- Stark (accessibility)
- Contrast Checker (color contrast)

---

**Document Owner**: UI/UX Engineer  
**Contributors**: Frontend Engineer, Product Owner  
**Last Updated**: November 12, 2025  
**Version**: 1.0.0  
**Status**: ✅ Complete - Living Document
