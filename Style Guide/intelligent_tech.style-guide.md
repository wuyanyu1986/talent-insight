# Intelligent Tech Style Guide

**Style Overview**:
A modern, sophisticated **light theme** for Web centered on tech cyan with soft multi-color blur gradients, using subtle surface color variations and gentle shadows to create refined visual hierarchy, complemented by moderate spacing for a professional, innovative, and intelligent aesthetic.

## Colors
### Primary Colors
  - **primary-base**: `text-[#2DD4BF]` or `bg-[#2DD4BF]` - Tech Cyan
  - **primary-lighter**: `bg-[#5EEAD4]`
  - **primary-darker**: `text-[#14B8A6]` or `bg-[#14B8A6]`
  - **primary-deep**: `text-[#0D9488]` or `bg-[#0D9488]`

### Background Colors

#### Structural Backgrounds

Choose based on layout type:

**For Vertical Layout** (Top Header + Optional Side Panels):
- **bg-nav-primary**: `style="background: radial-gradient(circle at 20% 50%, rgba(94, 234, 212, 0.03) 0%, transparent 50%), radial-gradient(circle at 80% 50%, rgba(148, 163, 184, 0.02) 0%, transparent 50%), #F8FAFB;"` - Top header with subtle gradient glow
- **bg-nav-secondary**: `style="background: radial-gradient(circle at 50% 0%, rgba(94, 234, 212, 0.025) 0%, transparent 60%), #FAFBFC;"` - Inner Left sidebar (if present)
- **bg-page**: `bg-[#FCFCFD]` - Page background (bg of Main Content area)

**For Horizontal Layout** (Side Navigation + Optional Top Bar):
- **bg-nav-primary**: `style="background: radial-gradient(circle at 50% 20%, rgba(94, 234, 212, 0.03) 0%, transparent 50%), radial-gradient(circle at 50% 80%, rgba(148, 163, 184, 0.02) 0%, transparent 50%), #F8FAFB;"` - Left main sidebar with subtle gradient glow
- **bg-nav-secondary**: `style="background: radial-gradient(circle at 0% 50%, rgba(94, 234, 212, 0.025) 0%, transparent 60%), #FAFBFC;"` - Inner Top header (if present)
- **bg-page**: `bg-[#FCFCFD]` - Page background (bg of Main Content area)

#### Container Backgrounds
For main content area. Adjust values when used on navigation backgrounds to ensure sufficient contrast.
- **bg-container-primary**: `bg-white/90`
- **bg-container-secondary**: `bg-white/70`
- **bg-container-elevated**: `bg-white` - For cards that need more prominence
- **bg-container-inset**: `bg-[#F0FDFA]` - Subtle cyan tint for highlighting
- **bg-container-inset-strong**: `bg-[#CCFBF1]` - Stronger cyan tint
- **bg-container-gradient-subtle**: `style="background: linear-gradient(135deg, rgba(94, 234, 212, 0.08) 0%, rgba(255, 255, 255, 0.5) 100%);"` - Soft gradient container

### Text Colors
- **color-text-primary**: `text-[#0F172A]`
- **color-text-secondary**: `text-[#475569]`
- **color-text-tertiary**: `text-[#64748B]`
- **color-text-quaternary**: `text-[#94A3B8]`
- **color-text-on-dark-primary**: `text-white/95` - Text on dark backgrounds and primary-base, accent-dark color surfaces
- **color-text-on-dark-secondary**: `text-white/75` - Text on dark backgrounds and primary-base, accent-dark color surfaces
- **color-text-link**: `text-[#14B8A6]` - Links, text-only buttons without backgrounds, and clickable text in tables

### Functional Colors
Use **sparingly** to maintain a modern and professional overall style. Used for the surfaces of text-only cards, simple cards, buttons, and tags.
  - **color-success-default**: #A7F3D0
  - **color-success-light**: #D1FAE5 - tag/label bg
  - **color-error-default**: #FECACA - alert banner bg
  - **color-error-light**: #FEE2E2 - tag/label bg
  - **color-warning-default**: #FDE68A - tag/label bg
  - **color-warning-light**: #FEF3C7 - tag/label bg, alert banner bg
  - **color-info-default**: #BAE6FD
  - **color-info-light**: #E0F2FE - tag/label bg

### Accent Colors
  - A secondary palette for occasional highlights and categorization. **Avoid overuse** to protect brand identity. Use **sparingly**.
  - **accent-teal-soft**: `text-[#5EEAD4]` or `bg-[#5EEAD4]` - Soft teal for highlights
  - **accent-blue-gray**: `text-[#94A3B8]` or `bg-[#94A3B8]` - Cool blue-gray for contrast
  - **accent-silver**: `text-[#CBD5E1]` or `bg-[#CBD5E1]` - Light silver for subtle elements
  - **accent-cyan-glow**: `style="background: linear-gradient(135deg, #5EEAD4 0%, #2DD4BF 100%);"` - Gradient for special emphasis

### Data Visualization Charts
For data visualization charts only.
  - Standard data colors: #2DD4BF, #5EEAD4, #99F6E4, #14B8A6, #0D9488, #0F766E
  - Secondary data colors: #94A3B8, #CBD5E1, #E2E8F0, #64748B, #475569, #334155
  - Accent highlights: #A7F3D0, #BAE6FD, #DBEAFE, #FDE68A

## Typography
- **Font Stack**:
  - **font-family-base**: `-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif` — For regular UI copy

- **Font Size & Weight**:
  - **Caption**: `text-sm font-normal` - 14px
  - **Body**: `text-base font-normal` - 16px
  - **Body Emphasized**: `text-base font-semibold` - 16px
  - **Card Title / Subtitle**: `text-lg font-semibold` - 18px
  - **Page Title**: `text-2xl font-semibold` - 24px
  - **Headline**: `text-3xl font-semibold` - 30px
  - **Display**: `text-4xl font-bold` - 36px

- **Line Height**: 1.5

## Border Radius
  - **Small**: 8px — Small elements, tags
  - **Medium**: 12px — Buttons, inputs
  - **Large**: 16px — Cards, containers
  - **XLarge**: 20px — Large feature cards
  - **Full**: full — Avatars, badges, pills

## Layout & Spacing
  - **Tight**: 8px - For closely related small internal elements, such as icons and text within buttons
  - **Compact**: 12px - For small gaps between small containers, such as a line of tags
  - **Standard**: 16px - For gaps between medium containers like list items
  - **Comfortable**: 24px - For gaps between related content sections
  - **Relaxed**: 32px - For gaps between large containers and major sections
  - **Section**: 48px - For major section divisions and page segments

## Create Boundaries (contrast of surface color, borders, shadows)
The interface uses a combination of subtle surface color contrast and refined shadows to create gentle yet clear visual hierarchy, with occasional borders for emphasis.

### Borders
  - **Case 1 (Default)**: No borders for most containers, relying on background color differentiation and shadows.
  - **Case 2 (Subtle Accent)**: For inputs, selected states, and emphasis
    - **Light**: 1px solid #E2E8F0 - Subtle border for inputs
    - **Medium**: 1px solid #CBD5E1 - Standard border
    - **Accent**: 1px solid #5EEAD4 - For active/focused states
    - **Primary**: 1px solid #2DD4BF - For strong emphasis

### Dividers
  - **Case 1**: Use background color changes between sections when possible.
  - **Case 2**: If needed for clear separation, `border-t` or `border-b` `border-[#E2E8F0]`.
  - **Case 3**: For subtle dividers in light containers, `border-[#F1F5F9]`.

### Shadows & Effects
  - **Case 1 (Minimal)**: `shadow-[0_1px_3px_rgba(15,23,42,0.04)]` - Very subtle depth for flat cards
  - **Case 2 (Soft)**: `shadow-[0_2px_8px_rgba(15,23,42,0.06)]` - Standard cards, containers
  - **Case 3 (Elevated)**: `shadow-[0_4px_16px_rgba(15,23,42,0.08)]` - Important cards, modals
  - **Case 4 (Prominent)**: `shadow-[0_8px_24px_rgba(15,23,42,0.10)]` - Floating panels, dropdowns
  - **Case 5 (Glow Effect)**: `shadow-[0_0_20px_rgba(94,234,212,0.15)]` - Special emphasis with cyan glow

## Visual Emphasis for Containers
When containers (tags, cards, list items, rows) need visual emphasis to indicate priority, status, or category, use the following techniques:

| Technique | Implementation Notes | Best For | Avoid |
|-----------|---------------------|----------|-------|
| Background Tint | Use gradient backgrounds or adjust opacity | Gentle emphasis, premium cards | Overuse of strong gradients |
| Border Highlight | Use thin border with accent colors (1px solid #5EEAD4) | Active/selected states, important boundaries | Heavy borders on large containers |
| Glow/Shadow Effect | Apply cyan glow shadow for premium feel | Featured content, hover states | Overuse that diminishes impact |
| Gradient Overlay | Subtle gradient from primary-lighter to transparent | Hero sections, featured cards | Large areas that compete with content |
| Status Tag/Label | Add colored tag/label inside container | Status indicators, categories | - |

## Assets
### Image

- For normal `<img>`: `object-cover`
- For `<img>` with:
  - Slight overlay: `object-cover brightness-95`
  - Medium overlay: `object-cover brightness-90`
  - Heavy overlay: `object-cover brightness-75`

### Icon

- Use Lucide icons from Iconify (outline style, modern aesthetic).
- To ensure an aesthetic layout, each icon should be centered in a square container, typically without a background, matching the icon's size.
- Use Tailwind font size to control icon size
- Example:
  ```html
  <div class="flex items-center justify-center bg-transparent w-5 h-5">
  <iconify-icon icon="lucide:sparkles" class="text-base"></iconify-icon>
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
  - **Graphic**: Use a simple, relevant icon (e.g., a `sparkles` or `brain-circuit` icon for AI platform, a `trending-up` icon for analytics app).

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
<body class="w-[1440px] min-h-[700px] font-['-apple-system',BlinkMacSystemFont,'Segoe UI',Roboto,'Helvetica Neue',Arial,sans-serif] leading-[1.5]">

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
<body class="w-[1440px] min-h-[700px] flex font-['-apple-system',BlinkMacSystemFont,'Segoe UI',Roboto,'Helvetica Neue',Arial,sans-serif] leading-[1.5]">

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

- **Button**: (Note: Use flex and items-center for the container)
  - Example 1 (Primary Button with gradient):
    - button: `flex items-center gap-2 px-5 py-2.5 rounded-xl text-white/95 font-semibold transition-all duration-200 hover:shadow-[0_0_20px_rgba(94,234,212,0.25)]` `style="background: linear-gradient(135deg, #2DD4BF 0%, #14B8A6 100%);"`
      - icon (optional): `w-5 h-5`
      - span(button copy): `whitespace-nowrap`
  - Example 2 (Secondary Button):
    - button: `flex items-center gap-2 px-5 py-2.5 rounded-xl bg-white border border-[#CBD5E1] text-[#475569] font-semibold hover:bg-[#F8FAFC] hover:border-[#94A3B8] transition-all duration-200`
      - icon (optional): `w-5 h-5`
      - span(button copy): `whitespace-nowrap`
  - Example 3 (Icon Button):
    - button: `flex items-center justify-center w-10 h-10 rounded-xl bg-[#F0FDFA] hover:bg-[#CCFBF1] transition-colors duration-200`
      - icon: `w-5 h-5 text-[#14B8A6]`
  - Example 4 (Text Button):
    - button: `flex items-center gap-1 text-[#14B8A6] font-semibold hover:text-[#0D9488] transition-colors duration-200`
      - span(button copy): `whitespace-nowrap`

- **Tag Group (Filter Tags)** (Note: `overflow-x-auto` and `whitespace-nowrap` are required)
  - container(scrollable): `flex gap-2 overflow-x-auto [&::-webkit-scrollbar]:hidden`
    - label (Tag item):
      - input: `type="radio" name="tag1" class="sr-only peer" checked`
      - div: `px-4 py-2 rounded-full bg-white border border-[#E2E8F0] text-[#64748B] peer-checked:bg-[#2DD4BF] peer-checked:text-white/95 peer-checked:border-[#2DD4BF] peer-checked:shadow-[0_0_12px_rgba(45,212,191,0.2)] hover:border-[#CBD5E1] transition-all duration-200 whitespace-nowrap cursor-pointer`

### Data Entry
- **Progress bars/Slider**: `h-2 rounded-full bg-[#E2E8F0]` with inner progress `bg-gradient-to-r from-[#2DD4BF] to-[#14B8A6] h-full rounded-full`

- **Checkbox**
  - label: `flex items-center gap-3 cursor-pointer group`
    - input: `type="checkbox" class="sr-only peer"`
    - div: `w-5 h-5 bg-white border-2 border-[#CBD5E1] rounded-md flex items-center justify-center peer-checked:bg-[#2DD4BF] peer-checked:border-[#2DD4BF] text-transparent peer-checked:text-white transition-all duration-200 group-hover:border-[#94A3B8]`
      - svg(Checkmark): `stroke="currentColor" stroke-width="3" fill="none" viewBox="0 0 24 24" class="w-3.5 h-3.5"`
        - path: `d="M5 13l4 4L19 7"`
    - span(text): `text-[#475569] group-hover:text-[#0F172A]`

- **Radio button**
  - label: `flex items-center gap-3 cursor-pointer group`
    - input: `type="radio" name="option1" class="sr-only peer"`
    - div: `w-5 h-5 bg-white border-2 border-[#CBD5E1] rounded-full flex items-center justify-center peer-checked:border-[#2DD4BF] text-transparent peer-checked:text-[#2DD4BF] transition-all duration-200 group-hover:border-[#94A3B8]`
      - svg(dot indicator): `fill="currentColor" viewBox="0 0 24 24" class="w-3 h-3"`
        - circle: `cx="12" cy="12" r="6"`
    - span(text): `text-[#475569] group-hover:text-[#0F172A]`

- **Switch/Toggle**
  - label: `flex items-center gap-3 cursor-pointer group`
    - div: `relative`
      - input: `type="checkbox" class="sr-only peer"`
      - div(Toggle track): `w-14 h-7 bg-[#E2E8F0] rounded-full peer-checked:bg-[#2DD4BF] transition-all duration-300`
      - div(Toggle thumb): `absolute top-0.5 left-0.5 w-6 h-6 bg-white rounded-full shadow-[0_2px_4px_rgba(0,0,0,0.1)] peer-checked:translate-x-7 transition-transform duration-300`
    - span(text): `text-[#475569] group-hover:text-[#0F172A]`

- **Select/Dropdown**
  - Select container: `flex items-center justify-between px-4 py-2.5 bg-white border border-[#E2E8F0] rounded-xl hover:border-[#CBD5E1] transition-colors duration-200 cursor-pointer`
    - text: `text-[#475569]`
    - Dropdown icon(square container): `flex items-center justify-center bg-transparent w-5 h-5`
      - icon: `lucide:chevron-down text-base text-[#94A3B8]`

- **Input Field**
  - container: `flex flex-col gap-2`
    - label: `text-sm font-semibold text-[#475569]`
    - input: `px-4 py-2.5 bg-white border border-[#E2E8F0] rounded-xl text-[#0F172A] placeholder:text-[#94A3B8] focus:outline-none focus:border-[#2DD4BF] focus:ring-2 focus:ring-[#2DD4BF]/20 transition-all duration-200`

### Container
- **Navigation Menu - horizontal**
    - Nav Container: `flex items-center justify-between w-full px-8 py-4`
    - Left Section: `flex items-center gap-10`
      - Logo: `flex items-center gap-2`
      - Menu Item: `flex items-center gap-2 px-3 py-2 text-[#475569] hover:text-[#2DD4BF] transition-colors duration-200 cursor-pointer`
    - Right Section: `flex items-center gap-4`
      - Menu Item: `flex items-center gap-2 px-3 py-2 text-[#475569] hover:text-[#2DD4BF] transition-colors duration-200 cursor-pointer`
      - Notification (if applicable): `relative flex items-center justify-center w-10 h-10 rounded-xl bg-[#F0FDFA] hover:bg-[#CCFBF1] transition-colors duration-200 cursor-pointer`
        - notification-icon: `w-5 h-5 text-[#14B8A6]`
        - badge (if has unread): `absolute -top-1 -right-1 w-5 h-5 rounded-full bg-gradient-to-br from-[#2DD4BF] to-[#14B8A6] flex items-center justify-center shadow-[0_2px_8px_rgba(45,212,191,0.3)]`
          - badge-count: `text-xs font-bold text-white`
      - Avatar(if applicable): `flex items-center gap-2 cursor-pointer group`
        - avatar-image: `w-10 h-10 rounded-full border-2 border-[#E2E8F0] group-hover:border-[#2DD4BF] transition-colors duration-200`
        - dropdown-icon (if applicable): `w-4 h-4 text-[#94A3B8] group-hover:text-[#2DD4BF]`

- **Card**
    - Example 1 (Elevated card with soft shadow):
        - Card: `bg-white rounded-2xl shadow-[0_2px_8px_rgba(15,23,42,0.06)] hover:shadow-[0_4px_16px_rgba(15,23,42,0.08)] transition-shadow duration-300 p-6 flex flex-col gap-4`
        - Image (if applicable): `rounded-xl w-full object-cover`
        - Text area: `flex flex-col gap-3`
          - card-title: `text-lg font-semibold text-[#0F172A]`
          - card-subtitle: `text-sm font-normal text-[#64748B]`
    - Example 2 (Gradient accent card):
        - Card: `rounded-2xl shadow-[0_2px_8px_rgba(15,23,42,0.06)] hover:shadow-[0_4px_16px_rgba(15,23,42,0.08)] transition-shadow duration-300 p-6 flex flex-col gap-4` `style="background: linear-gradient(135deg, rgba(94, 234, 212, 0.08) 0%, rgba(255, 255, 255, 0.9) 100%);"`
        - Text area: `flex flex-col gap-3`
          - card-title: `text-lg font-semibold text-[#0F172A]`
          - card-subtitle: `text-sm font-normal text-[#64748B]`
    - Example 3 (Horizontal card with image):
        - Card: `bg-white rounded-2xl shadow-[0_2px_8px_rgba(15,23,42,0.06)] hover:shadow-[0_4px_16px_rgba(15,23,42,0.08)] transition-shadow duration-300 flex gap-4 p-4`
        - Image: `rounded-xl h-full w-32 object-cover flex-shrink-0`
        - Text area: `flex flex-col gap-3 flex-1`
          - card-title: `text-lg font-semibold text-[#0F172A]`
          - card-subtitle: `text-sm font-normal text-[#64748B]`
    - Example 4 (Feature card with glow effect):
        - Card: `bg-white rounded-2xl shadow-[0_4px_16px_rgba(15,23,42,0.08)] hover:shadow-[0_0_24px_rgba(94,234,212,0.2)] transition-all duration-300 p-6 flex flex-col gap-4 border border-[#E2E8F0] hover:border-[#5EEAD4]`
        - Icon container (if applicable): `w-12 h-12 rounded-xl bg-gradient-to-br from-[#2DD4BF] to-[#14B8A6] flex items-center justify-center`
          - icon: `w-6 h-6 text-white`
        - Text area: `flex flex-col gap-3`
          - card-title: `text-lg font-semibold text-[#0F172A]`
          - card-description: `text-sm font-normal text-[#64748B]`

- **Data Table Row**
  - row: `flex items-center px-6 py-4 hover:bg-[#F8FAFC] transition-colors duration-200 border-b border-[#F1F5F9]`
  - cell: `flex-1 text-sm text-[#475569]`
  - cell emphasized: `flex-1 text-sm font-semibold text-[#0F172A]`

## Additional Notes

This style guide emphasizes modern sophistication through soft gradients, refined shadows, and intelligent use of the tech cyan primary color. The design balances professional credibility with innovative visual language appropriate for an AI-powered enterprise platform. Maintain generous whitespace and consistent application of subtle gradient effects to reinforce the intelligent tech aesthetic throughout all interfaces.

<colors_extraction>
#2DD4BF
#5EEAD4
#14B8A6
#0D9488
#F8FAFB
#FAFBFC
#FCFCFD
#FFFFFF
#FFFFFFE6
#FFFFFFB3
#FFFFFF00
#F0FDFA
#CCFBF1
#0F172A
#475569
#64748B
#94A3B8
#FFFFFFF2
#FFFFFFBF
#A7F3D0
#D1FAE5
#FECACA
#FEE2E2
#FDE68A
#FEF3C7
#BAE6FD
#E0F2FE
#99F6E4
#0F766E
#CBD5E1
#E2E8F0
#334155
#DBEAFE
#F1F5F9
#F8FAFC
radial-gradient(circle at 20% 50%, rgba(94, 234, 212, 0.03) 0%, transparent 50%), radial-gradient(circle at 80% 50%, rgba(148, 163, 184, 0.02) 0%, transparent 50%), #F8FAFB
radial-gradient(circle at 50% 0%, rgba(94, 234, 212, 0.025) 0%, transparent 60%), #FAFBFC
radial-gradient(circle at 50% 20%, rgba(94, 234, 212, 0.03) 0%, transparent 50%), radial-gradient(circle at 50% 80%, rgba(148, 163, 184, 0.02) 0%, transparent 50%), #F8FAFB
radial-gradient(circle at 0% 50%, rgba(94, 234, 212, 0.025) 0%, transparent 60%), #FAFBFC
linear-gradient(135deg, rgba(94, 234, 212, 0.08) 0%, rgba(255, 255, 255, 0.5) 100%)
linear-gradient(135deg, #5EEAD4 0%, #2DD4BF 100%)
linear-gradient(135deg, #2DD4BF 0%, #14B8A6 100%)
linear-gradient(to right, #2DD4BF, #14B8A6)
linear-gradient(to br, #2DD4BF, #14B8A6)
linear-gradient(135deg, rgba(94, 234, 212, 0.08) 0%, rgba(255, 255, 255, 0.9) 100%)
</colors_extraction>
