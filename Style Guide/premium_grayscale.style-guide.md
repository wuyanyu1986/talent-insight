# Premium Grayscale Style Guide

**Style Overview**:
An ultra-minimalist **light theme** built on sophisticated grayscale hierarchy, using subtle surface color differences and generous whitespace to create refined visual layers. This design embodies executive-level restraint with magazine-quality elegance, offering uncompromising professionalism for high-end enterprise applications.
Avoid gradients, borders, shadows, and any colors not defined in this style.

## Colors
### Primary Colors
  - **primary-base**: `text-[#2C2C2C]` or `bg-[#2C2C2C]`
  - **primary-lighter**: `bg-[#3F3F3F]`
  - **primary-darker**: `text-[#1A1A1A]` or `bg-[#1A1A1A]`

### Background Colors

#### Structural Backgrounds

Choose based on layout type:

**For Vertical Layout** (Top Header + Optional Side Panels):
- **bg-nav-primary**: `bg-[hsla(0, 0%, 97%, 1)]` - Top header
- **bg-nav-secondary**: `bg-[hsla(0, 0%, 99%, 1)]` - Inner Left sidebar (if present)
- **bg-page**: `bg-white` - Page background (bg of Main Content area)

**For Horizontal Layout** (Side Navigation + Optional Top Bar):
- **bg-nav-primary**: `bg-[hsla(0, 0%, 97%, 1)]` - Left main sidebar
- **bg-nav-secondary**: `bg-white` - Inner Top header (if present)
- **bg-page**: `bg-white` - Page background (bg of Main Content area)

#### Container Backgrounds
For main content area. Adjust values when used on navigation backgrounds to ensure sufficient contrast.
- **bg-container-primary**: `bg-[hsla(0, 0%, 98%, 1)]`
- **bg-container-secondary**: `bg-[hsla(0, 0%, 96%, 1)]`
- **bg-container-inset**: `bg-[hsla(0, 0%, 94%, 1)]`
- **bg-container-inset-strong**: `bg-[hsla(0, 0%, 91%, 1)]`

### Text Colors
- **color-text-primary**: `text-[#1A1A1A]`
- **color-text-secondary**: `text-[#4A4A4A]`
- **color-text-tertiary**: `text-[#787878]`
- **color-text-quaternary**: `text-[#A8A8A8]`
- **color-text-on-dark-primary**: `text-white/95` - Text on dark backgrounds and primary-base color surfaces
- **color-text-on-dark-secondary**: `text-white/75` - Text on dark backgrounds and primary-base color surfaces
- **color-text-link**: `text-[#2C2C2C] underline decoration-1 underline-offset-2` - Links, text-only buttons

### Functional Colors
Use **extremely sparingly** to maintain monochromatic purity. Only for critical status indication.
  - **color-success-default**: `bg-[#E8E8E8]` - Neutral gray for success states
  - **color-success-light**: `bg-[#F2F2F2]` - tag/label bg
  - **color-error-default**: `bg-[#D4D4D4]` - Subtle gray for alerts
  - **color-error-light**: `bg-[#E8E8E8]` - tag/label bg
  - **color-warning-default**: `bg-[#DCDCDC]` - tag/label bg
  - **color-warning-light**: `bg-[#ECECEC]` - tag/label bg, alert banner bg
  - **color-function-default**: `bg-[#3F3F3F]` - Dark gray for functional elements
  - **color-function-light**: `bg-[#C8C8C8]` - tag/label bg

### Data Visualization Charts
For data visualization charts only. Sophisticated grayscale palette.
  - Standard data colors: `#F5F5F5`, `#E0E0E0`, `#BDBDBD`, `#8A8A8A`, `#5A5A5A`, `#3F3F3F`
  - Important data emphasis: `#2C2C2C`, `#1A1A1A`, `#0D0D0D`

## Typography
- **Font Stack**:
  - **font-family-base**: `-apple-system, BlinkMacSystemFont, "Segoe UI", "Helvetica Neue", Arial, sans-serif` — For regular UI copy

- **Font Size & Weight**:
  - **Caption**: `text-sm font-normal`
  - **Body**: `text-base font-normal`
  - **Body Emphasized**: `text-base font-medium`
  - **Card Title / Subtitle**: `text-lg font-medium`
  - **Page Title**: `text-2xl font-medium`
  - **Headline**: `text-4xl font-light`

- **Line Height**: 1.6 — Enhanced readability for content-focused interfaces

## Border Radius
  - **Small**: 0px — Sharp edges maintain professional aesthetic
  - **Medium**: 0px
  - **Large**: 2px — Minimal softening for large cards
  - **Full**: full — Toggles, avatars, small tags only

## Layout & Spacing
  - **Tight**: 8px - For closely related small internal elements
  - **Compact**: 16px - For small gaps between small containers
  - **Standard**: 24px - For gaps between medium containers like list items
  - **Relaxed**: 40px - For gaps between large containers and sections
  - **Section**: 64px - For major section divisions with magazine-like breathing room

## Create Boundaries (contrast of surface color, borders, shadows)
Boundaries are created exclusively through **subtle surface color contrast** with no borders or shadows. The extremely refined grayscale hierarchy establishes visual separation through minute tonal shifts, embodying ultimate restraint.

### Borders
  - **Case**: No borders. Visual separation achieved purely through surface color differentiation.

### Dividers
  - **Case**: No dividers. Whitespace and surface color contrast define all boundaries.

### Shadows & Effects
  - **Case**: No shadows or effects. Pure surface-based hierarchy maintains uncompromising minimalism.

## Visual Emphasis for Containers
When containers (tags, cards, list items, rows) need visual emphasis to indicate priority, status, or category, use the following techniques:

| Technique | Implementation Notes | Best For | Avoid |
|-----------|---------------------|----------|-------|
| Background Tint | Slightly darker/lighter grayscale value | Primary emphasis technique for all scenarios | Any colors outside defined grayscale palette |
| Status Tag/Label | Add grayscale tag/label inside container | Larger containers requiring categorical distinction | - |

## Assets
### Image

- For normal `<img>`: `object-cover`
- For `<img>` with overlay (rare, use sparingly):
  - Slight overlay: `object-cover brightness-90 grayscale`
  - Heavy overlay: `object-cover brightness-75 grayscale`

### Icon
- Use Lucide icons from Iconify for their clean, minimalist aesthetic.
- To ensure an aesthetic layout, each icon should be centered in a square container, typically without a background, matching the icon's size.
- Use Tailwind font size to control icon size
- Example:
  ```html
  <div class="flex items-center justify-center bg-transparent w-5 h-5">
  <iconify-icon icon="lucide:briefcase" class="text-base"></iconify-icon>
  </div>
  ```

### Third-Party Brand Logos:
   - Use Brand Icons from Iconify.
   - Logo Example:
     Monochrome Logo: `<iconify-icon icon="simple-icons:linkedin"></iconify-icon>`
     Colored Logo: `<iconify-icon icon="logos:google-icon"></iconify-icon>`

### User's Own Logo:
- To protect copyright, do **NOT** use real product logos as a logo for a new product, individual user, or other company products.
- **Icon-based**:
  - **Graphic**: Use a simple, relevant icon (e.g., a `brain` icon for AI platform, a `target` icon for talent platform).

## Page Layout - Web (*EXTREMELY* important)
### Determine Layout Type
- Choose between Vertical or Horizontal layout based on whether the primary navigation is a full-width top header or a full-height sidebar (left/right).
- User requirements typically indicate the layout preference. If unclear, consider:
  - Marketing/content sites typically use Vertical Layout.
  - Functional/dashboard sites can use either, depending on visual style. Sidebars accommodate more complex navigation than top bars. For complex navigation needs with a preference for minimal chrome (Vertical Layout adds an extra fixed header), choose Horizontal Layout (omits the fixed top header).
- Vertical Layout Diagram:
┌──────────────────────────────────────────────────────┐
│  Header (Primary Nav)                                │
├──────────┬──────────────────────────────┬────────────┤
│Left      │ Sub-header (Tertiary Nav)    │ Right      │
│Sidebar   │ (optional)                   │ Sidebar    │
│(Secondary├──────────────────────────────┤ (Utility   │
│Nav)      │ Main Content                 │ Panel)     │
│(optional)│                              │ (optional) │
│          │                              │            │
└──────────┴──────────────────────────────┴────────────┘
- Horizontal Layout Diagram:
┌──────────┬──────────────────────────────┬───────────┐
│          │ Header (Secondary Nav)       │           │
│ Left     │ (optional)                   │ Right     │
│ Sidebar  ├──────────────────────────────┤ Sidebar   │
│ (Primary │ Main Content                 │ (Utility  │
│ Nav)     │                              │ Panel)    │
│          │                              │ (optional)│
│          │                              │           │
└──────────┴──────────────────────────────┴───────────┘
### Detailed Layout Code
**Vertical Layout**
```html
<!-- Body: Adjust width (w-[1440px]) based on target screen size -->
<body class="w-[1440px] min-h-[700px] font-[-apple-system,BlinkMacSystemFont,'Segoe UI','Helvetica Neue',Arial,sans-serif] leading-[1.6]">

  <!-- Header (Primary Nav): Fixed height -->
  <header class="w-full">
    <!-- Header content -->
  </header>

  <!-- Content Container: Must include 'flex' class -->
  <div class="w-full flex min-h-[700px]">
    <!-- Left Sidebar (Secondary Nav) (Optional): Remove if not needed. If Left Sidebar exists, use its ml to control left page margin -->
    <aside class="flex-shrink-0 min-w-fit">

    </aside>

    <!-- Main Content Area:
     Use Main Content Area's horizontal padding (px) to control distance from main content to sidebars or page edges.
     For pages without sidebars (like Marketing Pages, simple content pages such as help centers, privacy policies) use larger values (px-30 to px-80), for pages with sidebars (Functional/Dashboard Pages, complex content pages with multi-level navigation like knowledge base articles) use moderate values (px-8 to px-16) -->
    <main class="flex-1 overflow-x-hidden flex flex-col">
    <!--  Main Content -->

    </main>

    <!-- Right Sidebar (Utility Panel) (Optional): Remove if not needed. If Right Sidebar exists, use its mr to control right page margin -->
    <aside class="flex-shrink-0 min-w-fit">
    </aside>

  </div>
</body>
```

**Horizontal Layout**

```html
<!-- Body: Adjust width (w-[1440px]) based on target screen size. Must include 'flex' class -->
<body class="w-[1440px] min-h-[700px] flex font-[-apple-system,BlinkMacSystemFont,'Segoe UI','Helvetica Neue',Arial,sans-serif] leading-[1.6]">

<!-- Left Sidebar (Primary Nav): Use its ml to control left page margin -->
  <aside class="flex-shrink-0 min-w-fit">
  </aside>

  <!-- Content Container-->
  <div class="flex-1 overflow-x-hidden flex flex-col min-h-[700px]">

    <!-- Header (Secondary Nav) (Optional): Remove if not needed. If Header exists, use its mx to control distance to left/right sidebars or page margins -->
    <header class="w-full">
    </header>

    <!-- Main Content Area: Use Main Content Area's pl to control distance from main content to left sidebar. Use pr to control distance to right sidebar/right page edge -->
    <main class="w-full">
    </main>


  </div>

  <!-- Right Sidebar (Utility Panel) (Optional): Remove if not needed. If Right Sidebar exists, use its mr to control right page margin -->
  <aside class="flex-shrink-0 min-w-fit">
  </aside>

</body>
```

## Tailwind Component Examples (Key attributes)
**Important Note**: Use utility classes directly. Do NOT create custom CSS classes or add styles in <style> tags for the following components

### Basic

- **Button**:
  - Example 1 (text button):
    - button: `flex items-center gap-2 px-6 py-3 bg-[#2C2C2C] text-white/95 hover:bg-[#1A1A1A] transition-colors`
      - span(button copy): `whitespace-nowrap text-sm font-medium tracking-wide`
  - Example 2 (icon button):
    - button: `flex items-center justify-center w-10 h-10 bg-[hsla(0,0%,94%,1)] hover:bg-[hsla(0,0%,91%,1)] transition-colors`
      - icon

- **Tag Group (Filter Tags)**
  - container(scrollable): `flex gap-3 overflow-x-auto [&::-webkit-scrollbar]:hidden`
    - label (Tag item):
      - input: `type="radio" name="tag1" class="sr-only peer" checked`
      - div: `px-4 py-2 bg-[hsla(0,0%,96%,1)] text-[#4A4A4A] peer-checked:bg-[#2C2C2C] peer-checked:text-white/95 hover:bg-[hsla(0,0%,94%,1)] transition-colors whitespace-nowrap text-sm font-medium`

### Data Entry
- **Progress bars/Slider**: `h-1 bg-[hsla(0,0%,94%,1)]`
  - Fill: `bg-[#2C2C2C]`

- **Checkbox**
  - label: `flex items-center gap-3`
    - input: `type="checkbox" class="sr-only peer"`
    - div: `w-5 h-5 bg-[hsla(0,0%,94%,1)] flex items-center justify-center peer-checked:bg-[#2C2C2C] text-transparent peer-checked:text-white/95 transition-colors`
      - svg(Checkmark): `stroke="currentColor" stroke-width="3"`
    - span(text): `text-sm font-normal text-[#4A4A4A]`

- **Radio button**
  - label: `flex items-center gap-3`
    - input: `type="radio" name="option1" class="sr-only peer"`
    - div: `w-5 h-5 bg-[hsla(0,0%,94%,1)] rounded-full flex items-center justify-center peer-checked:bg-[#2C2C2C] text-transparent peer-checked:text-white/95 transition-colors`
      - svg(dot indicator): `fill="currentColor" w-2 h-2`
    - span(text): `text-sm font-normal text-[#4A4A4A]`

- **Switch/Toggle**
  - label: `flex items-center gap-3`
    - div: `relative`
      - input: `type="checkbox" class="sr-only peer"`
      - div(Toggle track): `w-11 h-6 bg-[hsla(0,0%,94%,1)] peer-checked:bg-[#2C2C2C] transition-colors rounded-full`
      - div(Toggle thumb): `absolute top-0.5 left-0.5 w-5 h-5 bg-white rounded-full peer-checked:translate-x-5 transition-transform`
    - span(text): `text-sm font-normal text-[#4A4A4A]`

- **Select/Dropdown**
  - Select container: `flex items-center gap-2 px-4 py-2 bg-[hsla(0,0%,96%,1)]`
    - text: `text-sm font-normal text-[#4A4A4A]`
    - Dropdown icon(square container): `flex items-center justify-center bg-transparent w-5 h-5`
      - icon: `text-base text-[#787878]`

### Container
- **Navigation Menu - horizontal**
  - Nav Container: `flex items-center justify-between w-full px-12 py-6`
  - Left Section: `flex items-center gap-12`
    - Menu Item: `flex items-center gap-2 text-sm font-medium text-[#4A4A4A] hover:text-[#1A1A1A] transition-colors`
  - Right Section: `flex items-center gap-6`
    - Menu Item: `flex items-center gap-2 text-sm font-medium text-[#4A4A4A]`
    - Notification (if applicable): `relative flex items-center justify-center w-10 h-10`
      - notification-icon: `w-5 h-5 text-[#4A4A4A]`
      - badge (if has unread): `absolute -top-1 -right-1 w-5 h-5 rounded-full bg-[#2C2C2C] flex items-center justify-center`
        - badge-count: `text-xs text-white/95`
    - Avatar(if applicable): `flex items-center gap-2`
      - avatar-image: `w-9 h-9 rounded-full grayscale`
      - dropdown-icon (if applicable): `w-4 h-4 text-[#787878]`

- **Card**
  - Example 1 (Vertical card with image and text):
    - Card: `bg-[hsla(0,0%,98%,1)] rounded-sm flex flex-col p-6 gap-5`
    - Image: `w-full aspect-video object-cover grayscale`
    - Text area: `flex flex-col gap-3`
      - card-title: `text-lg font-medium text-[#1A1A1A]`
      - card-subtitle: `text-sm font-normal text-[#787878]`
  - Example 2 (Horizontal card with image and text):
    - Card: `bg-[hsla(0,0%,98%,1)] rounded-sm flex gap-6 p-6`
    - Image: `w-40 h-full object-cover grayscale`
    - Text area: `flex flex-col gap-4 flex-1`
      - card-title: `text-lg font-medium text-[#1A1A1A]`
      - card-subtitle: `text-sm font-normal text-[#787878]`
  - Example 3 (Image-focused card):
    - Card: `flex flex-col gap-4`
    - Image: `w-full aspect-video object-cover grayscale`
    - Text area: `flex flex-col gap-2`
      - card-title: `text-lg font-medium text-[#1A1A1A]`
      - card-subtitle: `text-sm font-normal text-[#787878]`
  - Example 4 (text-only cards, minimal):
    - Card: `bg-[hsla(0,0%,96%,1)] rounded-sm flex flex-col p-8 gap-6`

## Additional Notes
- **Grayscale Commitment**: All images should use `grayscale` filter to maintain monochromatic purity
- **Typography Hierarchy**: Relies on weight variation (light/normal/medium) and size rather than color
- **Interaction States**: Use subtle background tint shifts for hover/active states
- **Content Density**: Generous whitespace is essential - never compromise breathing room for information density
- **Professional Polish**: Every element should feel considered, refined, and intentional

<colors_extraction>
#2C2C2C
#3F3F3F
#1A1A1A
#F7F7F7
#FCFCFC
#FFFFFF
#FAFAFA
#F5F5F5
#F0F0F0
#E8E8E8
#4A4A4A
#787878
#A8A8A8
#FFFFFFF2
#FFFFFFBF
#E8E8E8
#F2F2F2
#D4D4D4
#DCDCDC
#ECECEC
#C8C8C8
#E0E0E0
#BDBDBD
#8A8A8A
#5A5A5A
#0D0D0D
</colors_extraction>
