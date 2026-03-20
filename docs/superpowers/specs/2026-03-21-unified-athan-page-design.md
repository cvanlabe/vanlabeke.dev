# Unified Athan Page Design

## Summary

Redesign the existing Athan macOS product page (`/athan/index.html`) into a unified "Athan family" page that showcases both the macOS and mobile (iOS & Android) versions of the app. A tab-based platform switcher allows visitors to toggle between macOS and mobile content, with deep-linkable URLs for App Store and Play Store marketing.

## Context

- Athan macOS is an existing product with a full product page at `/athan/`
- Athan Mobile is a new, separate app available on iOS and Android
- Feature parity is 100% between platforms, with one exception: the mobile app includes a Qibla compass screen not available on macOS
- Both apps share the same icon and visual identity (navy + gold theme)
- Pricing model is the same (free with optional one-time calendar booking purchase), though mobile may go fully free initially to build a customer base
- Each app has its own App Store / Play Store listing

## Design

### Tab System & Deep Linking

- A pill-shaped platform toggle in the nav bar with two options: **macOS** and **Mobile**
- Tab state is controlled via a query parameter: `?platform=macos` or `?platform=mobile`. This preserves hash-based section anchors (`#features`, `#pricing`, etc.) for in-page navigation. Default is macOS if no parameter is present.
- On page load, JS reads the query parameter and activates the correct tab.
- A `popstate` event listener keeps tab state in sync with browser back/forward navigation.
- Switching tabs updates the URL via `history.pushState` without page reload.
- Transitions between tabs use a smooth crossfade, no jarring layout shifts.
- App Store / Play Store marketing URLs can link directly: `/athan/?platform=mobile#download`

**DOM strategy:** Platform-specific sections are duplicated as sibling containers with `data-platform="macos"` and `data-platform="mobile"` attributes. A CSS class on `<body>` (e.g., `body.platform-macos` or `body.platform-mobile`) controls visibility via CSS selectors. Shared sections have no `data-platform` attribute and are always visible. This keeps the logic declarative and avoids JS string-building.

**Nav CTA ("Download"):** Remains a scroll link to the `#download` section, which itself swaps content per tab.

**Responsive behavior:** At narrow viewports (< 600px), the nav links are already hidden. The pill toggle remains in the nav bar since there is room alongside the brand. If it becomes tight, it can drop below the nav as a sticky bar.

**Accessibility:** The tab switcher follows the WAI-ARIA Tabs pattern: `role="tablist"` on the container, `role="tab"` on each option with `aria-selected`, `role="tabpanel"` on content regions with `aria-controls`/`aria-labelledby`. Arrow keys switch between tabs.

**SEO note:** Query-parameter-based tabs are not separately indexable by search engines — both platform views share one URL for ranking purposes. This is acceptable for now. A future path-based approach (`/athan/macos/`, `/athan/mobile/`) could be considered if organic search traffic per platform becomes a goal.

### Section Breakdown

#### 1. Hero (swaps per tab)

**macOS tab:**
- Current content preserved: app icon, "Prayer times, right where you need them"
- "Built for macOS" badge
- Mac App Store download button
- "Requires macOS 14 Sonoma or later"

**Mobile tab:**
- Same app icon
- Headline rewritten for mobile context (e.g., "Prayer times, wherever you go")
- "Built for iOS & Android" badge
- Two download buttons side by side: App Store + Google Play
- Requirements note (e.g., "Requires iOS 17 / Android 10 or later")
- Same star field background for both tabs

#### 2. Features Grid (swaps per tab)

**macOS tab:**
- 6 feature cards (current features, copy adjusted to be less macOS-specific where possible)
- Menu bar screenshot

**Mobile tab:**
- 7 feature cards (same 6 + Qibla compass card)
- Prayer times mobile screenshot

Feature card copy is rewritten to be platform-appropriate for each tab.

#### 3. Calendar USP / Booking Wizard (swaps screenshot per tab)

- Copy is platform-neutral: "your calendar" (not "macOS Calendar" or "phone's calendar")
- macOS tab: current macOS booking screenshot (`screenshot-book-prayers.png`)
- Mobile tab: mobile booking screenshot (`screen_book_prayers.png`)
- Feature bullet list stays the same for both, with one adjustment: "Drag-and-drop to manually adjust any proposed slot" becomes "Adjust any proposed slot" (interaction method differs per platform)

#### 4. How It Works (swaps per tab)

- 3 steps with the same structure, but copy tailored per platform
- macOS tab: retains specificity ("menu bar", "Mac App Store")
- Mobile tab: mobile-appropriate language ("home screen", "App Store or Google Play")
- This avoids watered-down generic copy and keeps each version compelling

#### 5. Settings (swaps per tab)

**macOS tab:**
- Current two macOS settings screenshots side by side

**Mobile tab:**
- `screen_settings.PNG` (Location, Calculation, Jumu'ah) + `screen_settings2.PNG` (Calendar, Display, Notifications) side by side

#### 6. Calculation Methods (shared, no changes)

- 12 methods grid, identical across platforms

#### 7. Pricing (shared, platform-neutral)

- Same pricing model, platform-neutral copy
- Download buttons in pricing cards follow the active tab's platform (Mac App Store for macOS tab, App Store + Google Play for mobile tab)

#### 8. CTA / Download (swaps per tab)

- macOS tab: Mac App Store button, macOS requirements badge
- Mobile tab: App Store + Google Play buttons, mobile requirements badge

#### 9. Testimonial (shared, no changes)

#### 10. Footer (shared)

- Single privacy policy link

### Page Metadata Update

- `<title>` updated from "Athan -- Prayer Times for Your Mac Menu Bar" to "Athan -- Prayer Times for macOS, iOS & Android"
- `<meta name="description">` updated to reflect multi-platform availability

### Privacy Policy Update

`/athan/privacy.html` is rewritten to be platform-neutral:

- Opening statement covers all platforms: "Athan is available on macOS, iOS, and Android. This policy applies to all versions."
- "macOS Core Location framework" becomes "your device's location services"
- "macOS Calendar" becomes "your device's calendar"
- "macOS UserDefaults" becomes "local device storage"
- "System Settings" becomes "your device's settings"
- All other macOS-specific references replaced with generic equivalents
- Verify whether "zero network requests" claim (current line 207) holds for mobile apps; adjust if needed

### Main Site Update

`/index.html` — the Athan card in the Tools section updates its platform label from "macOS" to "macOS / iOS / Android".

## Assets

### Existing macOS screenshots (unchanged)
- `screenshot-menubar.png`
- `screenshot-book-prayers.png`
- `screenshot-settings-calculation.png`
- `screenshot-settings-display.png`

### New mobile screenshots
- `screen_prayer_times.png` — prayer times list
- `screen_qibla.png` — Qibla compass
- `screen_book_prayers.png` — calendar booking view
- `screen_settings.png` — settings part 1 (Location, Calculation, Jumu'ah)
- `screen_settings2.png` — settings part 2 (Calendar, Display, Notifications)

**Note:** Mobile screenshots currently use uppercase `.PNG` extension on disk. Rename all to lowercase `.png` during implementation to avoid case-sensitivity issues on Linux-based hosting (GitHub Pages, Netlify, etc.).

### Store URLs

- macOS App Store: `https://apps.apple.com/us/app/athan-for-mac/id6760509284`
- iOS App Store: TBD (provide before implementation)
- Google Play Store: TBD (provide before implementation)

## What stays the same

- Visual identity: navy + gold theme, same typography, same scroll animations
- Page lives at `/athan/` — no new directories needed
- Same CSS file (extended, not replaced)
- App icon shared across both platforms
