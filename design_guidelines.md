# Design Guidelines: Community-Driven Disaster Reporting Portal

## Design Approach

**System-Based with Emergency UI Patterns**  
Draw from Material Design's information density principles combined with emergency management interface patterns (inspired by crisis dashboards like Crisis24, FEMA apps, and emergency services platforms). Prioritize clarity, scanability, and immediate comprehension over decorative elements.

**Core Principle**: Every design decision optimizes for speed of reporting and clarity of information under high-stress conditions.

---

## Typography System

**Font Stack**: Inter (primary), SF Pro (fallback) via Google Fonts CDN

**Hierarchy**:
- **Page Headers**: 3xl to 4xl weight-700 for dashboard sections
- **Card Titles/Report Headers**: xl to 2xl weight-600 
- **Body Text**: base weight-400 for descriptions, report details
- **Meta Information**: sm weight-500 for timestamps, locations, categories
- **Labels/Tags**: xs to sm weight-600 uppercase with letter-spacing for status indicators
- **CTAs**: base to lg weight-600 for primary actions

**Critical**: Use tabular numbers for timestamps, coordinates, and severity scores to maintain visual alignment.

---

## Layout System

**Spacing Primitives**: Tailwind units of 2, 4, 6, and 8 as foundation
- Component padding: p-4 to p-6
- Section spacing: space-y-6 to space-y-8
- Card gaps: gap-4 to gap-6
- Container margins: mx-4 to mx-8

**Grid Architecture**:
- **Dashboard**: Two-column split on desktop (lg:grid-cols-3) - 2/3 for map, 1/3 for report feed
- **Report Form**: Single column max-w-2xl centered with generous spacing
- **Report Cards**: Grid of 1 column mobile, 2 columns tablet (md:grid-cols-2)
- **Mobile**: Always stack to single column, prioritize map visibility

**Container Strategy**:
- Full-width map container with absolute positioning for overlay controls
- Constrained content areas: max-w-7xl for dashboard, max-w-2xl for forms
- Fixed header navigation: sticky top-0 for quick access to report button

---

## Component Library

### Navigation Header
- **Structure**: Sticky horizontal bar with logo left, "Submit Report" CTA right
- **Height**: h-16 to h-20 for touch-friendly targets
- **Mobile**: Hamburger menu with slide-in drawer for filtering options
- **Report Button**: Prominent floating action button (FAB) on mobile, fixed bottom-right

### Hero Section (Dashboard View)
- **Layout**: Full-width split - Interactive map (65-70% width) + Live feed sidebar (30-35%)
- **Map Container**: min-h-screen minus header, relative positioning for marker overlays
- **Overlay Controls**: Absolute positioned filter chips (top-left), legend (bottom-left)
- **No decorative hero** - immediately show functional dashboard

### Report Submission Form
- **Layout**: Centered card design, max-w-2xl, generous p-8 padding
- **Structure**: Vertical stack with logical grouping
  - Location selector (map pin or coordinates)
  - Disaster type dropdown with icons
  - Urgency level selector (radio cards, visually prominent)
  - Description textarea (min 4 rows)
  - Photo upload zone (drag-drop area with preview)
  - AI analysis toggle (optional real-time severity check)
- **Field spacing**: space-y-6 between form groups
- **Button bar**: Sticky bottom bar on mobile, inline on desktop

### Report Cards (Feed)
- **Card Structure**: 
  - Header: Icon + Category badge + Timestamp (flex justify-between)
  - Body: Location (with pin icon) + Description preview (2-3 lines, truncated)
  - Footer: Severity indicator + Action buttons (View Details, Mark Safe)
- **Sizing**: p-4 to p-6, rounded-lg borders
- **Spacing**: space-y-4 in vertical feed
- **Interaction**: Hover lift effect (subtle shadow increase), click expands details

### Map Markers
- **Visual Design**: Icon-based markers with severity rings
- **Sizes**: Different scales for zoom levels (w-8 h-8 to w-12 h-12)
- **Clustering**: Number badges for grouped incidents
- **Active State**: Pulse animation on newly added reports (first 60 seconds)

### Status Badges
- **Urgency Levels**: Pill-shaped badges with icons
  - Critical, High, Medium, Low, Info
- **Typography**: xs to sm weight-600 uppercase
- **Spacing**: px-3 py-1 for comfortable touch targets
- **Icons**: Leading icon from Heroicons (fire, exclamation, info-circle)

### Filter Chips
- **Layout**: Horizontal scroll on mobile, wrapped flex on desktop
- **Structure**: Icon + Label with close button
- **Sizing**: px-4 py-2, rounded-full
- **Interaction**: Toggle selection, removable via X icon

### AI Analysis Panel
- **Display**: Expandable accordion or modal overlay
- **Content**: Severity assessment + Suggested actions list + Confidence score
- **Layout**: Three-section vertical stack with dividers
- **Typography**: Hierarchy from assessment (lg weight-600) to actions (base)

---

## Page Structures

### Dashboard (Main View)
1. **Sticky Header**: Logo + Active filters + Submit Report CTA
2. **Main Grid**: Map (2/3) + Report Feed (1/3) - stacks on mobile
3. **Filter Bar**: Horizontal chips above map for disaster types
4. **Map Legend**: Positioned bottom-left, collapsible on mobile
5. **Real-time Counter**: Top-right overlay showing "X reports in last hour"

### Submit Report Page
1. **Progress Breadcrumb**: Step indicator (1/3 steps if multi-step)
2. **Form Card**: Centered, max-w-2xl
3. **Live Preview**: Optional sidebar showing how report will appear
4. **Success Modal**: Full-screen confirmation with report ID and map preview

### Report Detail Modal/Page
1. **Header**: Category icon + Title + Timestamp + Status badge
2. **Image Gallery**: Full-width carousel if multiple photos
3. **Details Grid**: Two-column layout for metadata (location, reporter, severity)
4. **AI Analysis**: Expandable section with insights
5. **Action Bar**: Verify, Share, Mark Safe buttons
6. **Similar Reports**: Carousel of nearby incidents

---

## Images

**Hero/Dashboard**: No traditional hero image - lead directly with functional map interface

**Report Cards**: User-submitted incident photos (square thumbnails, 1:1 ratio, rounded corners)

**Empty States**: Illustration for "No reports in your area" - simple icon-based graphic

**Form Upload Zone**: Dashed border placeholder with upload icon, transforms to image preview on selection

**Map Markers**: Use icon library (Heroicons) for disaster type indicators, no custom images needed

---

## Accessibility & Emergency Considerations

- **High Contrast**: All text meets WCAG AA against backgrounds
- **Touch Targets**: Minimum 44x44px (h-11, w-11) for all interactive elements
- **Keyboard Navigation**: Full tab support, visible focus rings (ring-2 ring-offset-2)
- **Screen Readers**: ARIA labels for map markers, status indicators, and dynamic content updates
- **Offline Indicators**: Clear visual feedback when connectivity is lost
- **Loading States**: Skeleton screens for map loading, spinner for form submission

---

## Animation Strategy

**Minimal & Purposeful**:
- Map marker pulse (2s duration) for new reports only
- Smooth transitions (transition-all duration-200) for card hovers
- Slide-in animation for mobile drawer menu
- Success checkmark animation on form submission
- **NO** parallax, scroll-triggered animations, or decorative motion

---

## Icon Library

**Heroicons (Outline + Solid variants)** via CDN:
- Map pin, fire, water, medical, warning, check, x-circle, clock, users, photo, filter, menu

---

## Mobile-First Priorities

1. **Single-tap reporting**: FAB always visible for quick access
2. **Map-first view**: Maximize map real estate on small screens
3. **Swipeable cards**: Horizontal swipe to reveal actions on report cards
4. **Bottom sheet**: Filters and details slide up from bottom on mobile
5. **Geolocation**: Auto-detect user location for faster reporting