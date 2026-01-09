# TomOS Launcher — Product Specification

## Overview

**TomOS Launcher** is a lightweight, fast app launcher that lives within the TomOS ecosystem. It provides quick access to frequently used apps without the cognitive overhead of searching through home screens or the App Library.

### Why a Separate App?

The core TomOS App is focused on **capture and productivity** — a deliberate, focused experience. A launcher has the opposite purpose: **quick dispatch** — get in, tap, get out. Combining them would create UX friction and dilute both experiences.

TomOS Launcher is a sibling app, sharing the TomOS design language but with a distinct, speed-optimised purpose.

---

## Product Principles

1. **Sub-second interaction** — Open → Tap → Gone
2. **Zero configuration required** — Works out of the box with sensible defaults
3. **Configurable for power users** — But never forced
4. **Native feel** — Looks and feels like it belongs on iOS
5. **Minimal footprint** — Small app size, instant launch

---

## Core Features

### MVP (v1.0)

| Feature | Description | Priority |
|---------|-------------|----------|
| **App Grid** | Configurable grid of app shortcuts | P0 |
| **Quick Launch** | Tap to open app via URL scheme | P0 |
| **Widget** | Home screen widget with app grid | P0 |
| **App Picker** | Add/remove apps from the grid | P0 |
| **Reorder** | Drag to reorder apps | P1 |
| **Groups/Folders** | Organize apps into groups | P2 |

### Future (v1.x+)

| Feature | Description |
|---------|-------------|
| **Smart Suggestions** | Surface apps based on time/location |
| **TomOS Integration** | Deep link into TomOS quick capture |
| **Shortcuts Integration** | Launch Shortcuts, not just apps |
| **Recent Apps** | Show recently launched apps |
| **Search** | Filter apps by typing |

---

## User Experience

### Launch Flow

```
┌─────────────────────────────────────────┐
│          TomOS Launcher App             │
│                                         │
│   ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐      │
│   │     │ │     │ │     │ │     │      │
│   │ 📱  │ │ 🎵  │ │ 📧  │ │ 📅  │      │
│   │     │ │     │ │     │ │     │      │
│   └─────┘ └─────┘ └─────┘ └─────┘      │
│   Slack   Spotify  Mail   Calendar      │
│                                         │
│   ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐      │
│   │     │ │     │ │     │ │     │      │
│   │ 💬  │ │ 📝  │ │ ⚡  │ │ ➕  │      │
│   │     │ │     │ │     │ │     │      │
│   └─────┘ └─────┘ └─────┘ └─────┘      │
│  Messages TomOS  Shortcuts  Add...      │
│                                         │
│              ┌─────────┐                │
│              │  Edit   │                │
│              └─────────┘                │
└─────────────────────────────────────────┘
```

### Widget

```
┌─────────────────────────────────────────┐
│  TomOS Launch                    ⚙️     │
│                                         │
│   ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐      │
│   │ 📱  │ │ 🎵  │ │ 📧  │ │ 📅  │      │
│   └─────┘ └─────┘ └─────┘ └─────┘      │
│                                         │
│   ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐      │
│   │ 💬  │ │ 📝  │ │ ⚡  │ │ ➕  │      │
│   └─────┘ └─────┘ └─────┘ └─────┘      │
└─────────────────────────────────────────┘
```

Widget sizes:
- **Small**: 2x2 grid (4 apps)
- **Medium**: 4x2 grid (8 apps)
- **Large**: 4x4 grid (16 apps)

---

## Technical Architecture

### Stack

| Component | Technology |
|-----------|------------|
| **Framework** | SwiftUI |
| **Minimum iOS** | iOS 17.0 |
| **Data Storage** | SwiftData (local) |
| **Widget** | WidgetKit |
| **App Launch** | URL Schemes |

### URL Scheme Approach

iOS apps register URL schemes that allow other apps to open them:

```swift
// Open Spotify
if let url = URL(string: "spotify://") {
    await UIApplication.shared.open(url)
}

// Open Slack
if let url = URL(string: "slack://") {
    await UIApplication.shared.open(url)
}

// Open TomOS (deep link to quick capture)
if let url = URL(string: "tomos://capture") {
    await UIApplication.shared.open(url)
}
```

### Common App URL Schemes

| App | URL Scheme |
|-----|------------|
| Safari | `https://` or `x-web-search://` |
| Mail | `mailto:` |
| Messages | `sms:` |
| Phone | `tel:` |
| Calendar | `calshow://` |
| Maps | `maps://` |
| Music | `music://` |
| Spotify | `spotify://` |
| Slack | `slack://` |
| WhatsApp | `whatsapp://` |
| Telegram | `telegram://` |
| Notes | `mobilenotes://` |
| Reminders | `x-apple-reminderkit://` |
| Settings | `App-prefs://` |
| Shortcuts | `shortcuts://` |
| Files | `shareddocuments://` |

### Data Model

```swift
import SwiftData

@Model
class LaunchItem {
    var id: UUID
    var name: String
    var urlScheme: String
    var iconName: String?        // SF Symbol name (fallback)
    var customIconData: Data?    // User-provided icon
    var position: Int
    var groupId: UUID?
    var isEnabled: Bool
    var createdAt: Date
    var lastLaunchedAt: Date?
    var launchCount: Int
    
    init(name: String, urlScheme: String) {
        self.id = UUID()
        self.name = name
        self.urlScheme = urlScheme
        self.position = 0
        self.isEnabled = true
        self.createdAt = Date()
        self.launchCount = 0
    }
}

@Model
class LaunchGroup {
    var id: UUID
    var name: String
    var position: Int
    var color: String?          // Hex color for group indicator
    
    init(name: String) {
        self.id = UUID()
        self.name = name
        self.position = 0
    }
}
```

### App Detection

To help users add apps, we can check if URL schemes are registered:

```swift
func canOpenApp(scheme: String) -> Bool {
    guard let url = URL(string: scheme) else { return false }
    return UIApplication.shared.canOpenURL(url)
}
```

Note: iOS requires declaring queried schemes in `Info.plist`:

```xml
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>spotify</string>
    <string>slack</string>
    <string>whatsapp</string>
    <!-- ... other schemes ... -->
</array>
```

---

## UI Components

### App Tile

```swift
struct AppTile: View {
    let item: LaunchItem
    let size: CGFloat
    
    var body: some View {
        Button(action: launch) {
            VStack(spacing: 8) {
                // Icon
                RoundedRectangle(cornerRadius: size * 0.22)
                    .fill(Color(.secondarySystemBackground))
                    .frame(width: size, height: size)
                    .overlay {
                        if let iconName = item.iconName {
                            Image(systemName: iconName)
                                .font(.system(size: size * 0.4))
                                .foregroundStyle(.secondary)
                        }
                    }
                
                // Label
                Text(item.name)
                    .font(.caption)
                    .lineLimit(1)
            }
        }
        .buttonStyle(.plain)
    }
    
    private func launch() {
        guard let url = URL(string: item.urlScheme) else { return }
        UIApplication.shared.open(url)
    }
}
```

### App Grid

```swift
struct LauncherGrid: View {
    @Query(sort: \LaunchItem.position) var items: [LaunchItem]
    
    let columns = [
        GridItem(.flexible()),
        GridItem(.flexible()),
        GridItem(.flexible()),
        GridItem(.flexible())
    ]
    
    var body: some View {
        ScrollView {
            LazyVGrid(columns: columns, spacing: 20) {
                ForEach(items) { item in
                    AppTile(item: item, size: 60)
                }
                
                // Add button
                AddAppButton()
            }
            .padding()
        }
    }
}
```

### Edit Mode

- Long press on grid enters edit mode
- Tiles wiggle (like iOS home screen)
- Drag to reorder
- Tap X to remove
- Tap "Done" to exit

---

## Widget Implementation

### Widget Configuration

```swift
struct LauncherWidget: Widget {
    let kind: String = "LauncherWidget"
    
    var body: some WidgetConfiguration {
        StaticConfiguration(kind: kind, provider: Provider()) { entry in
            LauncherWidgetView(entry: entry)
        }
        .configurationDisplayName("TomOS Launch")
        .description("Quick access to your apps")
        .supportedFamilies([.systemSmall, .systemMedium, .systemLarge])
    }
}
```

### Widget Deep Links

Widgets can't directly open URL schemes, so they deep link into the app which then opens the target:

```swift
// Widget link
Link(destination: URL(string: "tomos-launcher://open?app=spotify")!) {
    AppTileWidget(item: item)
}

// App handles the deep link
func handleDeepLink(_ url: URL) {
    guard url.scheme == "tomos-launcher",
          url.host == "open",
          let appScheme = url.queryParameters["app"],
          let targetUrl = URL(string: "\(appScheme)://") else { return }
    
    UIApplication.shared.open(targetUrl)
}
```

---

## Settings

### Configuration Options

| Setting | Type | Default |
|---------|------|---------|
| Grid columns | 3 / 4 / 5 | 4 |
| Show labels | Bool | true |
| Haptic feedback | Bool | true |
| App icon style | Rounded / Circle / Square | Rounded |
| Theme | System / Light / Dark | System |

### Settings View

Minimal — accessible via gear icon, not a prominent feature.

---

## Onboarding

### First Launch Flow

1. **Welcome screen** — "TomOS Launcher" + tagline
2. **Quick setup** — "Add your first apps" with popular suggestions
3. **Widget prompt** — "Add to Home Screen for fastest access"
4. **Done** — Drops into main grid

Keep it under 30 seconds.

---

## Integration Points

### TomOS App

- Launcher can include TomOS quick capture as a "tile"
- URL scheme: `tomos://capture`
- Provides fast access to capture without opening full app

### Shortcuts

- Support Shortcuts as tiles (not just apps)
- URL scheme: `shortcuts://run-shortcut?name=ShortcutName`

### Backend (Optional, Future)

- Could sync launcher config via TomOS API
- Share launcher setup across devices
- Not required for MVP

---

## Design Specifications

### Following TomOS Design System

| Element | Specification |
|---------|---------------|
| **App accent** | Slate (`#64748B`) — utility feel |
| **Background** | System background (adapts to light/dark) |
| **Tile size** | 60pt (app), adaptive for widget |
| **Corner radius** | 13pt (matches iOS app icons) |
| **Spacing** | 16pt grid padding, 20pt between tiles |
| **Typography** | SF Pro, Caption for labels |

### Visual Hierarchy

1. App icons dominate
2. Labels secondary (optional to hide)
3. Chrome minimal (Edit button, Settings gear)

---

## Development Phases

### Phase 1: Core (Week 1-2)
- [ ] Project setup (SwiftUI, SwiftData)
- [ ] Basic app grid view
- [ ] URL scheme launching
- [ ] Add/remove apps
- [ ] Persist with SwiftData

### Phase 2: Polish (Week 3)
- [ ] Edit mode with reorder
- [ ] Settings screen
- [ ] Haptic feedback
- [ ] Basic onboarding

### Phase 3: Widget (Week 4)
- [ ] WidgetKit integration
- [ ] Small/Medium/Large sizes
- [ ] Deep link handling

### Phase 4: Ship (Week 5)
- [ ] TestFlight distribution
- [ ] App Store assets (if publishing)
- [ ] Documentation

---

## Open Questions

1. **App icon discovery**: Should we include a built-in database of common app URL schemes, or rely on user to add manually?

2. **Icon fetching**: iOS doesn't provide a public API to fetch other apps' icons. Options:
   - SF Symbols as fallback (current plan)
   - User provides screenshot/image
   - Build a database of app icons (maintenance burden)

3. **Folder/Group UI**: How should groups be displayed? Accordion? Separate pages? iOS-style folder popup?

4. **Widget editability**: Should widget be configurable independently of the main app grid, or always mirror it?

---

## Success Metrics

| Metric | Target |
|--------|--------|
| Launch → app open | < 1 second |
| App size | < 10 MB |
| Daily active widget users | Primary success indicator |
| Crash-free sessions | > 99.5% |

---

## References

- [Apple URL Schemes](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app)
- [WidgetKit Documentation](https://developer.apple.com/documentation/widgetkit)
- [TomOS Design System](/design/DESIGN-SYSTEM.md)
- [TomOS App CLAUDE.md](/projects/tomos-app/CLAUDE.md)

---

*Last updated: January 2026*
*Version: 1.0*
*Status: Specification — Ready for Development*
