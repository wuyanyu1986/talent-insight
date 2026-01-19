# Modern Business Blue Style Guide

**Style Overview**:
A flat, professional **light theme** centered on deep business blue, using layered background colors and subtle contrast to create clear visual hierarchy. Designed for enterprise-grade web applications with emphasis on data clarity, readability, and comfortable long-duration usage.
Avoid gradients, shadows, and any colors not defined in this style.

## Colors
### Primary Colors
  - **primary-base**: `text-[#1E3A5F]` or `bg-[#1E3A5F]`
  - **primary-lighter**: `bg-[#2E4A6F]`
  - **primary-darker**: `text-[#152942]` or `bg-[#152942]`

### Background Colors

#### Structural Backgrounds

Choose based on layout type:

**For Vertical Layout** (Top Header + Optional Side Panels):
- **bg-nav-primary**: `bg-[#FAFBFC]` - Top header
- **bg-nav-secondary**: `bg-[#F5F7FA]` - Inner Left sidebar (if present)
- **bg-page**: `bg-[#FAFBFC]` - Page background (bg of Main Content area)

**For Horizontal Layout** (Side Navigation + Optional Top Bar):
- **bg-nav-primary**: `bg-[#F5F7FA]` - Left main sidebar
- **bg-nav-secondary**: `bg-[#FAFBFC]` - Inner Top header (if present)
- **bg-page**: `bg-[#FAFBFC]` - Page background (bg of Main Content area)

#### Container Backgrounds
For main content area. Adjust values when used on navigation backgrounds to ensure sufficient contrast.
- **bg-container-primary**: `bg-white`
- **bg-container-secondary**: `bg-[#F8F9FB]`
- **bg-container-inset**: `bg-[#EDF1F7]`
- **bg-container-inset-strong**: `bg-[#E1E8F0]`

### Text Colors
- **color-text-primary**: `text-[#0A1929]`
- **color-text-secondary**: `text-[#4A5568]`
- **color-text-tertiary**: `text-[#718096]`
- **color-text-quaternary**: `text-[#A0AEC0]`
- **color-text-on-dark-primary**: `text-white/95` - Text on dark backgrounds and primary-base color surfaces
- **color-text-on-dark-secondary**: `text-white/75` - Text on dark backgrounds and primary-base color surfaces
- **color-text-link**: `text-[#2E4A6F]` - Links, text-only buttons without backgrounds, and clickable text in tables

### Functional Colors
Use **sparingly** to maintain a minimalist and neutral overall style. Used for the surfaces of text-only cards, simple cards, buttons, and tags.
  - **color-success-default**: #34C759
  - **color-success-light**: #E8F8ED - tag/label bg
  - **color-error-default**: #E24C4B - alert banner bg
  - **color-error-light**: #FDEAEA - tag/label bg
  - **color-warning-default**: #F5A623 - alert banner bg
  - **color-warning-light**: #FEF6E7 - tag/label bg
  - **color-info-default**: #4A90E2
  - **color-info-light**: #EBF3FC - tag/label bg

### Accent Colors
  - A secondary palette for occasional highlights and categorization. **Avoid overuse** to protect brand identity. Use **sparingly**.
  - **accent-cyan-soft**: `text-[#5B9FBC]` or `bg-[#5B9FBC]`
  - **accent-steel-blue**: `text-[#6B7C93]` or `bg-[#6B7C93]`
  - **accent-teal-muted**: `text-[#4A8895]` or `bg-[#4A8895]`

### Data Visualization Charts
For data visualization charts only.
  - Standard data colors: #1E3A5F, #2E4A6F, #5B9FBC, #4A8895, #6B7C93, #8E9FB8
  - Important data can use small amounts of: #34C759, #F5A623, #E24C4B, #4A90E2

## Typography
- **Font Stack**:
  - **font-family-base**: `-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif` — For regular UI copy

- **Font Size & Weight**:

  - **Caption**: `text-sm font-normal`
  - **Body**: `text-base font-normal`
  - **Body Emphasized**: `text-base font-semibold`
  - **Card Title / Subtitle**: `text-lg font-semibold`
  - **Page Title**: `text-2xl font-semibold`
  - **Headline**: `text-3xl font-semibold`

- **Line Height**: 1.6

## Border Radius
  - **Small**: 6px — Elements inside cards (e.g., photos, icons)
  - **Medium**: 8px — Inputs, buttons, small cards
  - **Large**: 12px — Cards, containers
  - **Full**: full — Toggles, avatars, small tags

## Layout & Spacing
  - **Tight**: 8px - For closely related small internal elements, such as icons and text within buttons
  - **Compact**: 12px - For small gaps between small containers, such as a line of tags
  - **Standard**: 20px - For gaps between medium containers like list items
  - **Relaxed**: 32px - For gaps between large containers and sections
  - **Section**: 48px - For major section divisions


## Create Boundaries (contrast of surface color, borders, shadows)
Primarily relying on surface color contrast and clear borders to create boundaries. No shadows for a clean, flat aesthetic while maintaining professional hierarchy.

### Borders
  - **Default**: 1px solid #E2E8F0 (gray-200). Used for inputs, cards, table cells. `border border-[#E2E8F0]`
  - **Stronger**: 1px solid #CBD5E0 (gray-300). Used for active or focused states, section dividers. `border border-[#CBD5E0]`
  - **Subtle**: 1px solid #EDF2F7 (gray-100). Used for very light separations within containers. `border border-[#EDF2F7]`

### Dividers
  - Used generously to create clear sections in data-dense interfaces
  - Horizontal: `border-t border-[#E2E8F0]` or `border-b border-[#E2E8F0]`
  - Vertical: `border-l border-[#E2E8F0]` or `border-r border-[#E2E8F0]`

### Shadows & Effects
  - **No shadow** - Pure flat design for clean, professional appearance suitable for enterprise data applications

## Visual Emphasis for Containers
When containers (tags, cards, list items, rows) need visual emphasis to indicate priority, status, or category, use the following techniques:

| Technique | Implementation Notes | Best For | Avoid |
|-----------|---------------------|----------|-------|
| Background Tint | Slightly darker/lighter color or reduce transparency of backgrounds | Gentle, common approach for moderate emphasis needs | Heavy colors on large areas (e.g., red background for entire large cards) |
| Border Highlight | Use thin border with opacity for subtlety | Active/selected states, form validation | - |
| Status Tag/Label | Add colored tag/label inside container | Larger containers | - |
| Side Accent Bar | **Left edge only**, for **non-rounded containers** | Small non-rounded list items (e.g., side nav tabs), small non-rounded cards (e.g., task cards) | Large cards, wide list items, rounded containers |

## Assets
### Image

- For normal `<img>`: object-cover
- For `<img>` with:
  - Slight overlay: object-cover brightness-90
  - Heavy overlay: object-cover brightness-75

### Icon

- Use Lucide icons from Iconify.
- To ensure an aesthetic layout, each icon should be centered in a square container, typically without a background, matching the icon's size.
- Use Tailwind font size to control icon size
- Example:
  ```html
  <div class="flex items-center justify-center bg-transparent w-5 h-5">
  <iconify-icon icon="lucide:bar-chart-3" class="text-base"></iconify-icon>
  </div>
  ```

### Third-Party Brand Logos:
   - Use Brand Icons from Iconify.
   - Logo Example:
     Monochrome Logo: `<iconify-icon icon="simple-icons:x"></iconify-icon>`
     Colored Logo: `<iconify-icon icon="logos:google-icon"></iconify-icon>`

### User's Own Logo:
- To protect copyright, do **NOT** use real product logos as a logo for a new product, individual user, or other company products.
- **Icon-based**:
  - **Graphic**: Use a simple, relevant icon (e.g., a `users` icon for HR platform, a `brain` icon for AI analytics platform).

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
<body class="w-[1440px] min-h-[900px] font-[-apple-system,BlinkMacSystemFont,'Segoe_UI',Roboto,'Helvetica_Neue',Arial,sans-serif] leading-[1.6]">

  <!-- Header (Primary Nav): Fixed height -->
  <header class="w-full">
    <!-- Header content -->
  </header>

  <!-- Content Container: Must include 'flex' class -->
  <div class="w-full flex min-h-[900px]">
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
<body class="w-[1440px] min-h-[900px] flex font-[-apple-system,BlinkMacSystemFont,'Segoe_UI',Roboto,'Helvetica_Neue',Arial,sans-serif] leading-[1.6]">

<!-- Left Sidebar (Primary Nav): Use its ml to control left page margin -->
  <aside class="flex-shrink-0 min-w-fit">
  </aside>

  <!-- Content Container-->
  <div class="flex-1 overflow-x-hidden flex flex-col min-h-[900px]">

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

- **Button**: (Note: Use flex and items-center for the container)
  - Example 1 (Primary button):
    - button: flex items-center gap-2 bg-[#1E3A5F] text-white/95 px-5 py-2.5 rounded-lg hover:bg-[#152942] transition
      - icon (optional)
      - span(button copy): whitespace-nowrap text-base font-semibold
  - Example 2 (Secondary button):
    - button: flex items-center gap-2 bg-white border border-[#E2E8F0] text-[#1E3A5F] px-5 py-2.5 rounded-lg hover:bg-[#F8F9FB] transition
      - icon (optional)
      - span(button copy): whitespace-nowrap text-base font-semibold
  - Example 3 (Text button):
    - button: flex items-center gap-2 text-[#2E4A6F] hover:text-[#1E3A5F] transition
      - icon (optional)
      - span(button copy): whitespace-nowrap text-base font-semibold
  - Example 4 (Icon button):
    - button: flex items-center justify-center w-10 h-10 rounded-lg bg-white border border-[#E2E8F0] hover:bg-[#F8F9FB] transition
      - icon: text-lg text-[#4A5568]

- **Tag Group (Filter Tags)** (Note: `overflow-x-auto` and `whitespace-nowrap` are required)
  - container(scrollable): flex gap-3 overflow-x-auto [&::-webkit-scrollbar]:hidden
    - label (Tag item):
      - input: type="radio" name="tag1" class="sr-only peer" checked
      - div: px-4 py-2 rounded-lg bg-[#F8F9FB] text-[#4A5568] border border-[#E2E8F0] peer-checked:bg-[#1E3A5F] peer-checked:text-white/95 peer-checked:border-[#1E3A5F] hover:bg-[#EDF1F7] transition whitespace-nowrap text-sm font-medium

### Data Entry
- **Input Field**
  - container: flex flex-col gap-2
    - label: text-sm font-semibold text-[#0A1929]
    - input: w-full px-4 py-2.5 rounded-lg border border-[#E2E8F0] bg-white text-base text-[#0A1929] placeholder:text-[#A0AEC0] focus:outline-none focus:border-[#2E4A6F] focus:ring-2 focus:ring-[#2E4A6F]/20 transition

- **Progress bars/Slider**: h-2 rounded-full bg-[#EDF1F7]
  - progress fill: bg-[#1E3A5F] h-full rounded-full

- **Checkbox**
  - label: flex items-center gap-3 cursor-pointer
    - input: type="checkbox" class="sr-only peer"
    - div: w-5 h-5 bg-white rounded border-2 border-[#E2E8F0] flex items-center justify-center peer-checked:bg-[#1E3A5F] peer-checked:border-[#1E3A5F] transition
      - svg(Checkmark): stroke="currentColor" stroke-width="3" class="text-white w-3 h-3"
    - span(text): text-base text-[#0A1929]

- **Radio button**
  - label: flex items-center gap-3 cursor-pointer
    - input: type="radio" name="option1" class="sr-only peer"
    - div: w-5 h-5 bg-white rounded-full border-2 border-[#E2E8F0] flex items-center justify-center peer-checked:border-[#1E3A5F] transition
      - svg(dot indicator): fill="currentColor" class="text-[#1E3A5F] w-2.5 h-2.5 opacity-0 peer-checked:opacity-100 transition"
    - span(text): text-base text-[#0A1929]

- **Switch/Toggle**
  - label: flex items-center gap-3 cursor-pointer
    - div: relative
      - input: type="checkbox" class="sr-only peer"
      - div(Toggle track): w-11 h-6 bg-[#E2E8F0] peer-checked:bg-[#1E3A5F] rounded-full transition
      - div(Toggle thumb): absolute top-0.5 left-0.5 w-5 h-5 bg-white rounded-full peer-checked:translate-x-5 transition
    - span(text): text-base text-[#0A1929]

- **Select/Dropdown**
  - Select container: flex items-center gap-3 px-4 py-2.5 rounded-lg border border-[#E2E8F0] bg-white hover:bg-[#F8F9FB] cursor-pointer transition
    - text: text-base text-[#0A1929]
    - Dropdown icon(square container): flex items-center justify-center bg-transparent w-5 h-5
      - icon: text-base text-[#718096]


### Container
- **Navigation Menu - horizontal**
    - Navigation with sections/grouping:
        - Nav Container: flex items-center justify-between w-full px-8 py-4 bg-[#FAFBFC] border-b border-[#E2E8F0]
        - Left Section: flex items-center gap-8
          - Menu Item: flex items-center gap-2 text-base font-medium text-[#4A5568] hover:text-[#1E3A5F] transition cursor-pointer
            - icon (optional): w-5 h-5
            - text
        - Right Section: flex items-center gap-4
          - Menu Item: flex items-center gap-2
          - Notification (if applicable): relative flex items-center justify-center w-10 h-10 rounded-lg hover:bg-[#F8F9FB] transition cursor-pointer
            - notification-icon: w-5 h-5 text-[#4A5568]
            - badge (if has unread): absolute -top-1 -right-1 w-5 h-5 rounded-full bg-[#E24C4B] text-white flex items-center justify-center
              - badge-count: text-xs font-semibold
          - Avatar(if applicable): flex items-center gap-3 cursor-pointer
            - avatar-image: w-9 h-9 rounded-full border-2 border-[#E2E8F0]
            - dropdown-icon (if applicable): w-4 h-4 text-[#718096]

- **Card**
    - Example 1 (Data card with metrics):
        - Card: bg-white border border-[#E2E8F0] rounded-xl p-6 flex flex-col gap-4
        - Header: flex items-center justify-between
          - card-title: text-lg font-semibold text-[#0A1929]
          - icon: w-5 h-5 text-[#718096]
        - Metric area: flex flex-col gap-2
          - value: text-3xl font-semibold text-[#1E3A5F]
          - label: text-sm font-normal text-[#718096]
    - Example 2 (List card with items):
        - Card: bg-white border border-[#E2E8F0] rounded-xl p-6 flex flex-col gap-5
        - Card header: pb-4 border-b border-[#EDF2F7]
          - card-title: text-lg font-semibold text-[#0A1929]
        - List items: flex flex-col gap-0
          - item: py-4 border-b border-[#EDF2F7] last:border-0 flex items-center justify-between
    - Example 3 (Insight card with status):
        - Card: bg-white border border-[#E2E8F0] rounded-xl p-6 flex flex-col gap-4 hover:border-[#CBD5E0] transition cursor-pointer
        - Status indicator: flex items-center gap-2
          - status-dot: w-2 h-2 rounded-full bg-[#34C759]
          - status-text: text-sm font-medium text-[#4A5568]
        - Text area: flex flex-col gap-3
          - card-title: text-base font-semibold text-[#0A1929]
          - card-description: text-sm font-normal text-[#718096]

- **Table**
    - Table container: w-full bg-white border border-[#E2E8F0] rounded-xl overflow-hidden
      - Table: w-full
        - thead: bg-[#F8F9FB] border-b border-[#E2E8F0]
          - tr
            - th: px-6 py-4 text-left text-sm font-semibold text-[#4A5568]
        - tbody
          - tr: border-b border-[#EDF2F7] last:border-0 hover:bg-[#FAFBFC] transition
            - td: px-6 py-4 text-base text-[#0A1929]

## Additional Notes

This style guide is optimized for enterprise HR analytics platforms requiring extended viewing sessions. The flat design with clear borders ensures maximum clarity for data-dense interfaces while maintaining professional aesthetics. The carefully calibrated blue palette provides trust and stability without causing visual fatigue during prolonged use.

<colors_extraction>
#1E3A5F
#2E4A6F
#152942
#FAFBFC
#F5F7FA
#FFFFFF
#F8F9FB
#EDF1F7
#E1E8F0
#0A1929
#4A5568
#718096
#A0AEC0
#FFFFFFF2
#FFFFFFBF
#34C759
#E8F8ED
#E24C4B
#FDEAEA
#F5A623
#FEF6E7
#4A90E2
#EBF3FC
#5B9FBC
#6B7C93
#4A8895
#8E9FB8
#E2E8F0
#CBD5E0
#EDF2F7
</colors_extraction>
