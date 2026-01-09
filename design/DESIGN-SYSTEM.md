# TomOS Design System

The unified visual language for all TomOS Family applications.

---

## Principles

### 1. Calm Focus
- Reduce visual noise
- Clear hierarchy guides the eye
- White space is a feature, not waste

### 2. ADHD-Friendly
- Important actions are obvious
- Minimal decision fatigue
- Progress and state are always visible

### 3. Native Feel
- Follow Apple HIG patterns
- SF Symbols for iconography
- System fonts where appropriate
- Respect platform conventions

### 4. Consistent but Contextual
- Shared language across apps
- Each app can have personality within the system
- TomOS App = productivity, focused
- TomOS Launcher = speed, utility
- Notorious DAD = playful, music-forward

---

## Foundations

### Colors

#### Primary Palette

| Name | Hex | Usage |
|------|-----|-------|
| **Tom Blue** | `#007AFF` | Primary actions, links, key UI elements |
| **Tom Blue Dark** | `#0056CC` | Hover/pressed states |
| **Tom Blue Light** | `#4DA3FF` | Backgrounds, subtle accents |

#### Semantic Colors

| Name | Light Mode | Dark Mode | Usage |
|------|------------|-----------|-------|
| **Background Primary** | `#FFFFFF` | `#000000` | Main background |
| **Background Secondary** | `#F2F2F7` | `#1C1C1E` | Cards, grouped content |
| **Background Tertiary** | `#E5E5EA` | `#2C2C2E` | Nested elements |
| **Label Primary** | `#000000` | `#FFFFFF` | Main text |
| **Label Secondary** | `#3C3C43` (60%) | `#EBEBF5` (60%) | Secondary text |
| **Label Tertiary** | `#3C3C43` (30%) | `#EBEBF5` (30%) | Placeholder, disabled |
| **Separator** | `#C6C6C8` | `#38383A` | Dividers, borders |

#### Status Colors

| Name | Hex | Usage |
|------|-----|-------|
| **Success** | `#34C759` | Completed, positive |
| **Warning** | `#FF9500` | Attention needed |
| **Error** | `#FF3B30` | Errors, destructive |
| **Info** | `#5856D6` | Informational |

#### App Accent Colors (Optional)

Each app may define a unique accent for personality:

| App | Accent | Hex |
|-----|--------|-----|
| TomOS App | Tom Blue | `#007AFF` |
| TomOS Launcher | Slate | `#64748B` |
| Notorious DAD | Purple | `#AF52DE` |
| LegalOS | Forest | `#30D158` |

---

### Typography

#### Font Stack

- **Primary:** SF Pro (iOS/macOS system font)
- **Monospace:** SF Mono (code, technical content)
- **Fallback:** -apple-system, BlinkMacSystemFont, system-ui

#### Type Scale

| Style | Size | Weight | Line Height | Usage |
|-------|------|--------|-------------|-------|
| **Large Title** | 34pt | Bold | 41pt | Screen titles |
| **Title 1** | 28pt | Bold | 34pt | Section headers |
| **Title 2** | 22pt | Bold | 28pt | Card titles |
| **Title 3** | 20pt | Semibold | 25pt | Subsection headers |
| **Headline** | 17pt | Semibold | 22pt | Emphasized body |
| **Body** | 17pt | Regular | 22pt | Default text |
| **Callout** | 16pt | Regular | 21pt | Secondary body |
| **Subheadline** | 15pt | Regular | 20pt | Supporting text |
| **Footnote** | 13pt | Regular | 18pt | Metadata, timestamps |
| **Caption 1** | 12pt | Regular | 16pt | Labels, captions |
| **Caption 2** | 11pt | Regular | 13pt | Small labels |

#### SwiftUI Usage

```swift
Text("Title")
    .font(.largeTitle)
    
Text("Body text")
    .font(.body)
    
Text("12:34 PM")
    .font(.caption)
    .foregroundStyle(.secondary)
```

---

### Spacing

#### Base Unit
- **4pt grid** — all spacing should be multiples of 4

#### Standard Spacing Scale

| Token | Value | Usage |
|-------|-------|-------|
| `xxs` | 4pt | Tight spacing, inline elements |
| `xs` | 8pt | Related elements |
| `sm` | 12pt | Default component padding |
| `md` | 16pt | Card padding, section gaps |
| `lg` | 24pt | Section separation |
| `xl` | 32pt | Major section breaks |
| `xxl` | 48pt | Page-level spacing |

#### Standard Margins

| Context | Value |
|---------|-------|
| Screen edge padding | 16pt |
| Card internal padding | 16pt |
| List item vertical | 12pt |
| Between related buttons | 8pt |
| Between sections | 24pt |

---

### Iconography

#### SF Symbols
- Use SF Symbols exclusively
- Prefer filled variants for selected states
- Prefer outline variants for unselected states
- Use appropriate rendering mode (monochrome, hierarchical, palette, multicolor)

#### Standard Icon Sizes

| Context | Size | Weight |
|---------|------|--------|
| Tab bar | 24pt | Regular |
| Navigation bar | 22pt | Regular |
| List row accessory | 17pt | Regular |
| Inline with text | Match text size | Match text weight |
| Large display | 48-64pt | Light or Regular |

#### Common Icons

| Action | Symbol Name |
|--------|-------------|
| Add | `plus` / `plus.circle.fill` |
| Settings | `gearshape` / `gearshape.fill` |
| Search | `magnifyingglass` |
| Close | `xmark` / `xmark.circle.fill` |
| Back | `chevron.left` |
| Forward | `chevron.right` |
| More | `ellipsis` / `ellipsis.circle` |
| Share | `square.and.arrow.up` |
| Edit | `pencil` / `square.and.pencil` |
| Delete | `trash` / `trash.fill` |
| Done | `checkmark` / `checkmark.circle.fill` |
| Refresh | `arrow.clockwise` |

---

### Corner Radius

| Element | Radius |
|---------|--------|
| Buttons (small) | 8pt |
| Buttons (large) | 12pt |
| Cards | 12pt |
| Modal sheets | 16pt (top only) |
| Text fields | 8pt |
| Full-screen modals | 38pt (top only, matches iOS) |

---

### Shadows & Elevation

Use sparingly. Prefer separation via background color over shadows.

| Level | Usage | Shadow |
|-------|-------|--------|
| **0** | Flat, inline | None |
| **1** | Cards, raised | `0 1px 3px rgba(0,0,0,0.1)` |
| **2** | Floating, popovers | `0 4px 12px rgba(0,0,0,0.15)` |
| **3** | Modals | `0 8px 24px rgba(0,0,0,0.2)` |

---

## Components

### Buttons

#### Primary Button
- Background: Tom Blue
- Text: White, Semibold
- Height: 50pt
- Corner radius: 12pt
- Full width in forms, auto width in toolbars

```swift
Button("Continue") { }
    .buttonStyle(.borderedProminent)
    .controlSize(.large)
```

#### Secondary Button
- Background: Background Secondary
- Text: Tom Blue, Semibold
- Same dimensions as primary

#### Destructive Button
- Background: Error (or clear with Error text)
- Use sparingly, require confirmation

#### Icon Button
- 44pt minimum tap target
- Icon only, no label
- Use for toolbars, navigation

---

### Cards

- Background: Background Secondary
- Corner radius: 12pt
- Padding: 16pt
- No border in light mode; subtle border (`Separator`) optional in dark mode

```swift
VStack(alignment: .leading, spacing: 12) {
    Text("Card Title")
        .font(.headline)
    Text("Card content goes here")
        .font(.body)
        .foregroundStyle(.secondary)
}
.padding()
.background(Color(.secondarySystemBackground))
.clipShape(RoundedRectangle(cornerRadius: 12))
```

---

### Lists

- Use native `List` component
- Inset grouped style for settings/forms
- Plain style for content lists
- Swipe actions for quick operations

---

### Text Fields

- Use native `TextField` / `TextEditor`
- Clear button when populated
- Placeholder text in Label Tertiary
- Error state: red border + error message below

---

### Navigation

#### Tab Bar
- Maximum 5 items
- Use SF Symbols (filled when selected)
- Label below icon

#### Navigation Bar
- Large title on primary screens
- Inline title when scrolled or on secondary screens
- Back button auto-handled by NavigationStack

---

## Patterns

### Empty States
- Centered vertically
- Large SF Symbol (48-64pt, secondary color)
- Title (Title 2)
- Description (Body, secondary color)
- Primary action button

### Loading States
- Use native `ProgressView`
- Skeleton screens for content areas
- Never block entire UI if avoidable

### Error States
- Inline errors below relevant field
- Toast/banner for transient errors
- Full-screen error only for fatal issues
- Always provide recovery action

### Pull to Refresh
- Native `.refreshable` modifier
- Subtle haptic on trigger

---

## Motion

### Principles
- Motion should be purposeful
- Use spring animations for natural feel
- Keep durations short (0.2–0.4s)
- Reduce motion: respect system accessibility setting

### Standard Animations

| Type | Duration | Curve |
|------|----------|-------|
| Button press | 0.1s | easeOut |
| View transition | 0.3s | spring (response: 0.3, damping: 0.8) |
| Modal present | 0.35s | spring |
| Fade in/out | 0.2s | easeInOut |

```swift
withAnimation(.spring(response: 0.3, dampingFraction: 0.8)) {
    // state change
}
```

---

## Accessibility

### Minimum Requirements
- All interactive elements: 44pt minimum tap target
- Color contrast: WCAG AA minimum (4.5:1 for text)
- Dynamic Type: support all sizes
- VoiceOver: all elements labeled
- Reduce Motion: respect and provide alternatives

### Testing Checklist
- [ ] Works with Dynamic Type at all sizes
- [ ] VoiceOver navigation logical
- [ ] Sufficient color contrast
- [ ] Works with Bold Text enabled
- [ ] Works with Reduce Motion enabled

---

## Dark Mode

- Always support both modes
- Use semantic colors (not hardcoded)
- Test both modes during development
- Shadows may need adjustment in dark mode

---

## App-Specific Extensions

Individual apps may extend this system with:
- Custom accent color
- App-specific icons
- Unique components (documented locally)

Extensions must:
- Use the foundation tokens
- Follow the same principles
- Be documented in the app's local CLAUDE.md

---

*Last updated: January 2026*
*Version: 1.0*
