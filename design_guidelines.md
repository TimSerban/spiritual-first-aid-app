# Spiritual First Aid - Design Guidelines

## Design Approach
**System-Based with Brand Customization**: This is a utility-focused emergency response tool requiring clarity, reliability, and accessibility above all else. We'll use Material Design principles adapted to the established brand identity, prioritizing functional hierarchy and information density over decorative elements.

## Core Design Principles
1. **Calm Competency**: Restrained, professional interface that instills confidence during crisis
2. **Immediate Clarity**: No ambiguity in navigation, actions, or content hierarchy
3. **Accessibility-First**: WCAG AA+ compliance, large touch targets, high contrast throughout
4. **Offline Resilience**: Visual feedback for sync states, clear offline/online indicators

---

## Typography System

**Font Stack**:
- **UI/Body**: Inter (400, 500, 600, 700) - via Google Fonts CDN
- **Headings/Ritual Content**: Merriweather (400, 700) - via Google Fonts CDN

**Scale** (mobile → desktop):
- **Hero/H1**: text-3xl/text-4xl (Merriweather, font-bold)
- **H2**: text-2xl/text-3xl (Merriweather, font-bold) 
- **H3**: text-xl/text-2xl (Inter, font-semibold)
- **H4**: text-lg/text-xl (Inter, font-semibold)
- **Body**: text-base (Inter, font-normal, leading-relaxed for readability)
- **Small/Meta**: text-sm (Inter, font-medium)
- **Micro**: text-xs (Inter, font-medium, uppercase tracking-wide for labels)

**Emphasis**: Use Merriweather sparingly for gravitas (ritual scripts, section headings); Inter for all functional UI.

---

## Layout System

**Spacing Primitives**: Use Tailwind units of **2, 4, 8, 12, 16** for consistency
- Component padding: p-4, p-6, p-8
- Section spacing: space-y-8, space-y-12
- Card gaps: gap-4, gap-6
- Margins: m-2, m-4, m-8

**Grid & Containers**:
- Max content width: `max-w-6xl mx-auto` for readable line lengths
- Dashboard grids: `grid-cols-1 md:grid-cols-2 lg:grid-cols-3`
- Incident checklists: Single column (max-w-2xl) for focus
- Sidebar navigation: Fixed 16rem width on desktop, drawer on mobile

**Responsive Breakpoints**:
- Mobile-first approach, stack to single column below md:
- Touch targets minimum 44x44px throughout

---

## Component Library

### Navigation
- **Top Bar**: Fixed header with logo (compass+flame icon), tier badge, user menu, sync indicator
- **Sidebar** (Disaster Team/CPE Pro): Collapsible nav with Incident Mode, Drills, Resources, Team, Settings sections
- **Bottom Nav** (Mobile Free tier): 4-tab nav for core actions

### Cards & Containers
- **Default Card**: Subtle border, rounded-lg, p-6, shadow-sm on hover
- **Incident Card**: Elevated shadow-md, status indicator (dot + timestamp), quick actions in header
- **Checklist Items**: Large checkbox (w-6 h-6), clear label, optional assignee avatar, strikethrough when complete

### Buttons & Actions
**Primary CTA**: Solid background with brand colors, rounded-lg, px-6 py-3, font-semibold
**Secondary**: Border-2, transparent background, hover fill
**Destructive**: Use cautiously, always confirm (modal)
**Icon Buttons**: 44x44px minimum, tooltips on hover

### Forms & Inputs
- **Text Fields**: Outlined style, rounded-md, p-3, focus ring with accent color
- **Labels**: Above input, text-sm font-medium, mb-2
- **Helper Text**: text-xs below input for guidance
- **Validation**: Inline messages, clear error states with icons

### Modals & Overlays
- **Modal**: Centered, max-w-lg, backdrop blur, slide-up animation on mobile
- **Incident Mode Overlay**: Full-screen takeover with exit button, timer in header, step progress indicator

### Data Display
- **Tables**: Minimal borders, alternating row backgrounds for readability, sticky headers
- **Quiz Format**: Single question per screen, large radio buttons, immediate feedback panel
- **Notes List**: Timeline format with timestamps, export icon per note

### Icons
- **Library**: Heroicons (outline for nav, solid for actions) - via CDN
- **Sizes**: w-5 h-5 for inline, w-6 h-6 for buttons, w-8 h-8 for feature highlights
- **Custom Icons**: Use only for logo/compass+flame motif

---

## Specialized Modes

### Night Mode (Dark Theme)
- Background: #0B1220 → #1E293B gradient
- Text: #F1F5F9 primary, #CBD5E1 secondary
- Cards: #1E293B with subtle border
- Preserve color token accents for recognition

### Red-Light Mode (Field Use)
- Monochromatic red palette for night vision preservation
- Background: #1A0000, UI: #4A0000, Text: #FF6B6B
- Toggle in Settings, persists across sessions

### Offline Indicator
- Persistent banner when offline: "Working Offline" with sync icon
- Sync queue badge showing pending items
- Visual feedback when content is cached/available offline

---

## Tier Differentiation

**Visual Hierarchy**:
- **Free Community**: Access to core content, "Upgrade" prompts on locked features (blur preview + unlock CTA)
- **Disaster Spiritual Team**: Badge in header, unlocked team channels, full offline pack icon
- **Advanced CPE Pro**: Premium badge, supervisor dashboard access, cohort selector in nav

**Feature Gating**: Use lock icons + clear upgrade messaging, never hide navigation - show locked state

---

## Accessibility Mandates

- Minimum contrast ratio 7:1 for body text, 4.5:1 for large text
- All interactive elements keyboard-navigable with visible focus states
- ARIA labels on all icon-only buttons
- Form validation with screen-reader announcements
- Skip-to-content link for keyboard users
- No reliance on color alone for meaning

---

## Images

**Logo/Branding**: Compass + flame icon in header (SVG, dual-tone with brand colors)

**Content Illustrations**: Minimal, supportive icons for guidance modules (use Heroicons)

**No Hero Images**: This is a utility app - land directly into functionality or dashboard

**Avatars**: User/team member circles with initials fallback, uploaded photos optional

---

## Animations

**Minimal Motion**: 
- Page transitions: Simple fade (150ms)
- Checklist completion: Subtle checkmark animation (200ms)
- Loading states: Spinner or skeleton screens, no elaborate animations
- Respect `prefers-reduced-motion`

**Critical Animations**:
- Incident Mode activation: Screen takeover with clear entry/exit
- Sync indicator: Gentle pulse when syncing
- Quiz feedback: Immediate color shift (green/red) with brief icon pop

---

## Key Screens Layout

**Dashboard**: 3-column grid (lg:) showing active incidents, upcoming drills, recent notes - single column on mobile

**Incident Mode**: Full-screen checklist with timer in sticky header, progress bar, clear completion state

**Micro-Drill**: Card-based quiz UI, one question per view, navigation dots, results summary screen

**Notes Export**: Form with incident selector, date range, template picker, preview pane, export button

**Team Settings** (Disaster/Pro): Roster table with role dropdowns, invite input, seat count badge