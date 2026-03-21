# Unified Athan Page Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Redesign the existing Athan macOS product page into a unified page with a tab switcher for macOS and mobile (iOS & Android) platforms.

**Architecture:** Single HTML file (`/athan/index.html`) with inline CSS and vanilla JS. Platform-specific sections are duplicated as sibling containers with `data-platform` attributes. A CSS class on `<body>` controls visibility. Tab state uses query parameters (`?platform=macos|mobile`) to preserve hash anchors for section navigation.

**Tech Stack:** Static HTML, inline CSS, vanilla JavaScript. No build tools, no frameworks.

**Spec:** `docs/superpowers/specs/2026-03-21-unified-athan-page-design.md`

**Pre-implementation prerequisites:**
- iOS App Store URL and Google Play Store URL must be provided before deployment. Placeholder `#` links are used throughout this plan — search for `href="#"` and replace with actual URLs before going live.

---

## File Map

| File | Action | Responsibility |
|------|--------|----------------|
| `athan/index.html` | Rewrite | Unified product page with tab switcher |
| `athan/privacy.html` | Modify | Platform-neutral privacy policy |
| `index.html` | Modify | Update Athan card platform label |
| `athan/screen_*.PNG` | Rename | Lowercase `.png` extensions |

---

### Task 1: Rename mobile screenshots to lowercase extensions

**Files:**
- Rename: `athan/screen_prayer_times.PNG` -> `athan/screen_prayer_times.png`
- Rename: `athan/screen_qibla.PNG` -> `athan/screen_qibla.png`
- Rename: `athan/screen_book_prayers.PNG` -> `athan/screen_book_prayers.png`
- Rename: `athan/screen_settings.PNG` -> `athan/screen_settings.png`
- Rename: `athan/screen_settings2.PNG` -> `athan/screen_settings2.png`

- [ ] **Step 1: Rename all mobile screenshots to lowercase .png using git mv**

```bash
cd /Users/cvanlabe/Development/vanlabeke.dev/vanlabeke.dev
git mv athan/screen_prayer_times.PNG athan/screen_prayer_times.png
git mv athan/screen_qibla.PNG athan/screen_qibla.png
git mv athan/screen_book_prayers.PNG athan/screen_book_prayers.png
git mv athan/screen_settings.PNG athan/screen_settings.png
git mv athan/screen_settings2.PNG athan/screen_settings2.png
```

- [ ] **Step 2: Verify renames**

Run: `ls athan/screen_*.png`
Expected: All 5 files listed with lowercase `.png` extension.

- [ ] **Step 3: Commit**

```bash
git commit -m "chore: rename mobile screenshots to lowercase .png extensions"
```

---

### Task 2: Add tab system CSS to athan/index.html

Adds the CSS for the platform toggle pill, tab visibility logic, and crossfade transitions. Does not change any HTML yet.

**Files:**
- Modify: `athan/index.html` (CSS section, before `</style>`)

- [ ] **Step 1: Add platform toggle and tab visibility CSS**

Insert the following CSS before the closing `</style>` tag (after the `.fade-in.visible` rule, around line 839):

```css
/* Platform Toggle */
.platform-toggle {
    display: flex;
    background: var(--navy-mid);
    border: 1px solid rgba(201, 169, 97, 0.15);
    border-radius: 10px;
    padding: 3px;
    gap: 2px;
}

.platform-toggle button {
    background: none;
    border: none;
    color: var(--text-dim);
    padding: 0.4rem 1rem;
    border-radius: 8px;
    font-size: 0.85rem;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.2s;
    font-family: inherit;
}

.platform-toggle button[aria-selected="true"] {
    background: var(--gold);
    color: var(--navy);
    font-weight: 600;
}

.platform-toggle button:hover:not([aria-selected="true"]) {
    color: var(--gold);
}

/* Tab visibility */
[data-platform] {
    display: none;
}

body.platform-macos [data-platform="macos"],
body.platform-mobile [data-platform="mobile"] {
    display: block;
}

/* Restore grid/flex display for specific elements when visible */
body.platform-macos .features-layout[data-platform="macos"],
body.platform-mobile .features-layout[data-platform="mobile"] {
    display: grid;
}

body.platform-macos .hero-buttons[data-platform="macos"],
body.platform-mobile .hero-buttons[data-platform="mobile"] {
    display: flex;
}

body.platform-macos .settings-screenshots[data-platform="macos"],
body.platform-mobile .settings-screenshots[data-platform="mobile"] {
    display: grid;
}

body.platform-macos .pricing-cards[data-platform="macos"],
body.platform-mobile .pricing-cards[data-platform="mobile"] {
    display: grid;
}

body.platform-macos .steps[data-platform="macos"],
body.platform-mobile .steps[data-platform="mobile"] {
    display: grid;
}

/* Requirements badge (used for both macOS and mobile) */
.requirements-badge {
    display: inline-flex;
    align-items: center;
    gap: 0.4rem;
    font-size: 0.8rem;
    color: var(--text-dim);
    margin-top: 1rem;
}

/* Crossfade transition */
[data-platform] {
    animation: fadeIn 0.3s ease;
}

@keyframes fadeIn {
    from { opacity: 0; transform: translateY(6px); }
    to { opacity: 1; transform: translateY(0); }
}

/* Mobile screenshot styling (phone-sized images) */
.screenshot-mobile {
    max-width: 260px;
    margin: 0 auto;
    display: block;
    border-radius: 20px;
    border: 1px solid rgba(201, 169, 97, 0.1);
    box-shadow: 0 20px 60px rgba(0, 0, 0, 0.4);
}

/* Mobile feature screenshot in features-layout */
.features-screenshot-mobile {
    position: sticky;
    top: 6rem;
    text-align: center;
}

.features-screenshot-mobile img {
    max-width: 220px;
    border-radius: 20px;
    border: 1px solid rgba(201, 169, 97, 0.1);
    box-shadow: 0 20px 60px rgba(0, 0, 0, 0.4);
}

/* Google Play button style */
.btn-play {
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    background: var(--gold);
    color: var(--navy);
    padding: 0.875rem 2rem;
    border-radius: 12px;
    text-decoration: none;
    font-weight: 600;
    font-size: 1.05rem;
    transition: all 0.2s;
}

.btn-play:hover {
    background: var(--gold-light);
    transform: translateY(-2px);
    box-shadow: 0 8px 30px rgba(201, 169, 97, 0.3);
}
```

Also rename the existing `.macos-badge` CSS rule (around line 753 in the current file) to `.requirements-badge` — same styles, just a platform-neutral class name since it's used for both macOS and mobile requirements text.

- [ ] **Step 2: Verify page still loads correctly**

Open `athan/index.html` in a browser. The page should look identical since no HTML uses the new classes yet.

- [ ] **Step 3: Commit**

```bash
git add athan/index.html
git commit -m "style: add CSS for platform toggle, tab visibility, and mobile screenshots"
```

---

### Task 3: Add tab switcher JS to athan/index.html

Adds the JavaScript that reads query params, toggles body class, handles popstate, and wires up the toggle buttons.

**Files:**
- Modify: `athan/index.html` (script section, inside `<script>`)

- [ ] **Step 1: Add tab switching JS**

Insert the following JavaScript at the beginning of the existing `<script>` block (before the star field code):

```javascript
// Platform tab switching
function getPlatform() {
    const params = new URLSearchParams(window.location.search);
    return params.get('platform') === 'mobile' ? 'mobile' : 'macos';
}

function setPlatform(platform) {
    document.body.classList.remove('platform-macos', 'platform-mobile');
    document.body.classList.add('platform-' + platform);

    // Update toggle buttons
    document.querySelectorAll('.platform-toggle button').forEach(function(btn) {
        btn.setAttribute('aria-selected', btn.dataset.platform === platform);
    });

    // Update URL without reload
    var url = new URL(window.location);
    url.searchParams.set('platform', platform);
    history.pushState({ platform: platform }, '', url);
}

// Initialize on load
var initialPlatform = getPlatform();
document.body.classList.add('platform-' + initialPlatform);
history.replaceState({ platform: initialPlatform }, '', window.location);

// Handle browser back/forward
window.addEventListener('popstate', function(e) {
    var platform = (e.state && e.state.platform) ? e.state.platform : getPlatform();
    document.body.classList.remove('platform-macos', 'platform-mobile');
    document.body.classList.add('platform-' + platform);
    document.querySelectorAll('.platform-toggle button').forEach(function(btn) {
        btn.setAttribute('aria-selected', btn.dataset.platform === platform);
    });
});

// Wire up toggle buttons after DOM is ready
document.addEventListener('DOMContentLoaded', function() {
    document.querySelectorAll('.platform-toggle button').forEach(function(btn) {
        btn.addEventListener('click', function() {
            setPlatform(this.dataset.platform);
        });
        // Keyboard: arrow keys switch tabs
        btn.addEventListener('keydown', function(e) {
            if (e.key === 'ArrowLeft' || e.key === 'ArrowRight') {
                e.preventDefault();
                var buttons = Array.from(document.querySelectorAll('.platform-toggle button'));
                var idx = buttons.indexOf(this);
                var next = e.key === 'ArrowRight' ? (idx + 1) % buttons.length : (idx - 1 + buttons.length) % buttons.length;
                buttons[next].focus();
                setPlatform(buttons[next].dataset.platform);
            }
        });
    });

    // Set initial aria-selected state
    var currentPlatform = getPlatform();
    document.querySelectorAll('.platform-toggle button').forEach(function(btn) {
        btn.setAttribute('aria-selected', btn.dataset.platform === currentPlatform);
    });
});
```

- [ ] **Step 2: Verify JS loads without errors**

Open `athan/index.html` in browser. Open DevTools console. No errors should appear. The body should have class `platform-macos`.

- [ ] **Step 3: Commit**

```bash
git add athan/index.html
git commit -m "feat: add platform tab switching JS with query params and keyboard support"
```

---

### Task 4: Update nav with platform toggle

Replaces the nav HTML to include the pill-shaped platform toggle with ARIA attributes.

**Files:**
- Modify: `athan/index.html` (nav section)

- [ ] **Step 1: Replace the nav HTML**

Replace the existing `<nav>` block with:

```html
<!-- Nav -->
<nav>
    <a href="#" class="nav-brand">
        <img src="athan_web_icon.png" alt="Athan icon">
        Athan
    </a>
    <div class="platform-toggle" role="tablist" aria-label="Platform">
        <button role="tab" id="tab-macos" data-platform="macos" aria-selected="true" aria-controls="panel-macos" tabindex="0">macOS</button>
        <button role="tab" id="tab-mobile" data-platform="mobile" aria-selected="false" aria-controls="panel-mobile" tabindex="-1">iOS & Android</button>
    </div>
    <ul class="nav-links">
        <li><a href="#features">Features</a></li>
        <li><a href="#calendar">Calendar</a></li>
        <li><a href="#methods">Methods</a></li>
        <li><a href="#pricing">Pricing</a></li>
        <li><a href="#download" class="nav-cta">Download</a></li>
    </ul>
</nav>
```

- [ ] **Step 2: Verify toggle renders and switches**

Open in browser. The pill toggle should appear in the nav between the brand and the links. Clicking "iOS & Android" should add `platform-mobile` class to body and update the URL to `?platform=mobile`. Clicking "macOS" should revert.

- [ ] **Step 3: Commit**

```bash
git add athan/index.html
git commit -m "feat: add platform toggle to nav bar with ARIA tablist"
```

---

### Task 5: Update page metadata and hero with dual-platform content

Updates `<title>`, `<meta>`, and creates both macOS and mobile hero sections.

**Files:**
- Modify: `athan/index.html` (head and hero sections)

- [ ] **Step 1: Update title and add meta description**

Replace the existing `<title>` with:

```html
<title>Athan — Prayer Times for macOS, iOS & Android</title>
```

Add after the `<title>` tag:

```html
<meta name="description" content="Athan keeps you connected to your prayers throughout the day. Accurate prayer times, smart notifications, calendar booking, and a built-in Book of Prayers. Available on macOS, iOS, and Android.">
```

- [ ] **Step 2: Replace hero section with dual-platform heroes**

Replace the existing hero section with two hero content blocks — one for each platform. The star field and hero wrapper stay shared:

```html
<!-- Hero -->
<section class="hero" id="hero">
    <div class="stars" id="stars"></div>

    <div class="hero-content" data-platform="macos" role="tabpanel" aria-labelledby="tab-macos">
        <img src="athan_web_icon.png" alt="Athan app icon" class="hero-icon">
        <h1>Prayer times, right where you need them</h1>
        <div class="hero-badge">
            <span>&#9790;</span> Built for macOS
        </div>
        <p>Athan lives in your menu bar and keeps you connected to your prayers throughout the day. Accurate times, smart notifications, and calendar integration that works around your schedule.</p>
        <div class="hero-buttons">
            <a href="#download" class="btn-primary">
                <svg width="20" height="20" viewBox="0 0 20 20" fill="currentColor"><path d="M15.6 8.4c-.1-2.3 1.9-3.4 2-3.5-1.1-1.6-2.8-1.8-3.4-1.8-1.4-.1-2.8.9-3.5.9-.7 0-1.8-.8-3-.8-1.5 0-2.9.9-3.7 2.3-1.6 2.8-.4 6.9 1.1 9.1.8 1.1 1.7 2.3 2.9 2.3 1.1 0 1.6-.7 2.9-.7 1.4 0 1.8.7 3 .7 1.2 0 2-1.1 2.8-2.2.9-1.3 1.2-2.5 1.3-2.6-.1-.1-2.4-1-2.4-3.7zM13.4 2.8c.6-.8 1-1.8.9-2.8-.9 0-2 .6-2.6 1.4-.5.7-1 1.8-.9 2.8 1 .1 2-.5 2.6-1.4z"/></svg>
                Download on the App Store
            </a>
            <a href="#features" class="btn-secondary">See features</a>
        </div>
        <div class="requirements-badge">Requires macOS 14 Sonoma or later</div>
    </div>

    <div class="hero-content" data-platform="mobile" role="tabpanel" aria-labelledby="tab-mobile">
        <img src="athan_web_icon.png" alt="Athan app icon" class="hero-icon">
        <h1>Prayer times, wherever you go</h1>
        <div class="hero-badge">
            <span>&#9790;</span> Built for iOS & Android
        </div>
        <p>Athan keeps you connected to your prayers throughout the day. Accurate times, smart notifications, and calendar integration that works around your schedule — right in your pocket.</p>
        <div class="hero-buttons">
            <a href="#download" class="btn-primary">
                <svg width="20" height="20" viewBox="0 0 20 20" fill="currentColor"><path d="M15.6 8.4c-.1-2.3 1.9-3.4 2-3.5-1.1-1.6-2.8-1.8-3.4-1.8-1.4-.1-2.8.9-3.5.9-.7 0-1.8-.8-3-.8-1.5 0-2.9.9-3.7 2.3-1.6 2.8-.4 6.9 1.1 9.1.8 1.1 1.7 2.3 2.9 2.3 1.1 0 1.6-.7 2.9-.7 1.4 0 1.8.7 3 .7 1.2 0 2-1.1 2.8-2.2.9-1.3 1.2-2.5 1.3-2.6-.1-.1-2.4-1-2.4-3.7zM13.4 2.8c.6-.8 1-1.8.9-2.8-.9 0-2 .6-2.6 1.4-.5.7-1 1.8-.9 2.8 1 .1 2-.5 2.6-1.4z"/></svg>
                Download on the App Store
            </a>
            <a href="#download" class="btn-play">
                <svg width="20" height="20" viewBox="0 0 24 24" fill="currentColor"><path d="M3.609 1.814L13.792 12 3.61 22.186a.996.996 0 0 1-.61-.92V2.734a1 1 0 0 1 .609-.92zm10.89 10.893l2.302 2.302-10.937 6.333 8.635-8.635zm3.199-3.199l2.302 2.302a1 1 0 0 1 0 1.38l-2.302 2.302L15.395 12l2.303-2.492zM5.864 2.658L16.8 8.99l-2.302 2.302L5.864 2.658z"/></svg>
                Get it on Google Play
            </a>
            <a href="#features" class="btn-secondary">See features</a>
        </div>
        <div class="requirements-badge">Requires iOS 17 or later / Android 10 or later</div>
    </div>
</section>
```

- [ ] **Step 3: Verify both heroes display correctly**

Open in browser. The macOS hero should show by default. Add `?platform=mobile` to the URL — the mobile hero should show with two download buttons. Switch using the toggle.

- [ ] **Step 4: Commit**

```bash
git add athan/index.html
git commit -m "feat: add dual-platform hero section with metadata update"
```

---

### Task 6: Add dual-platform features section

Creates both macOS and mobile feature grids. macOS has 6 cards, mobile has 7 (adds Qibla).

**Files:**
- Modify: `athan/index.html` (features section)

- [ ] **Step 1: Replace features section with dual-platform content**

Replace the existing features section (`<section class="features" id="features">...</section>`) with:

```html
<!-- Features -->
<section class="features" id="features">
    <div class="container">
        <div class="section-header fade-in">
            <div class="section-label">Features</div>
            <h2 class="section-title">Everything you need. Nothing you don't.</h2>
            <p class="section-subtitle">Athan stays out of your way until it's time to pray. No bloat, no ads, no distractions.</p>
        </div>

        <!-- macOS Features -->
        <div class="features-layout" data-platform="macos">
            <div class="features-screenshot fade-in">
                <img src="screenshot-menubar.png" alt="Athan menu bar showing prayer times" class="screenshot screenshot-menubar">
            </div>
            <div class="features-grid">
                <div class="feature-card fade-in">
                    <div class="feature-icon gold">&#9790;</div>
                    <h3>Menu Bar Native</h3>
                    <p>Lives in your menu bar, always one glance away. Choose to see the next prayer, current prayer, countdown, or just the icon.</p>
                </div>
                <div class="feature-card fade-in">
                    <div class="feature-icon teal">&#9201;</div>
                    <h3>Accurate Prayer Times</h3>
                    <p>Powered by the Adhan library with 12 calculation methods used worldwide. Auto-detects the right method based on your location.</p>
                </div>
                <div class="feature-card fade-in">
                    <div class="feature-icon pink">&#128276;</div>
                    <h3>Smart Notifications</h3>
                    <p>Get notified when a prayer starts and set reminders before it ends. Configurable 15, 20, or 30 minute warnings so you never miss a prayer.</p>
                </div>
                <div class="feature-card fade-in">
                    <div class="feature-icon indigo">&#9745;</div>
                    <h3>Prayer Tracking</h3>
                    <p>Mark prayers as completed with a simple click. Resets daily at Fajr. A small thing that builds a big habit.</p>
                </div>
                <div class="feature-card fade-in">
                    <div class="feature-icon green">&#127759;</div>
                    <h3>Location Aware</h3>
                    <p>Uses your GPS location with reverse geocoding. Automatically updates prayer times when you travel. No manual city selection needed.</p>
                </div>
                <div class="feature-card fade-in">
                    <div class="feature-icon orange">&#128332;</div>
                    <h3>Jumu'ah Support</h3>
                    <p>Special handling for Friday prayers with configurable duration and optional fixed start times for your local mosque.</p>
                </div>
            </div>
        </div>

        <!-- Mobile Features -->
        <div class="features-layout" data-platform="mobile">
            <div class="features-screenshot-mobile fade-in">
                <img src="screen_prayer_times.png" alt="Athan mobile prayer times screen" class="screenshot-mobile">
            </div>
            <div class="features-grid">
                <div class="feature-card fade-in">
                    <div class="feature-icon gold">&#9790;</div>
                    <h3>Always Accessible</h3>
                    <p>Prayer times at your fingertips. Open the app to see today's full schedule with the current and next prayer highlighted.</p>
                </div>
                <div class="feature-card fade-in">
                    <div class="feature-icon teal">&#9201;</div>
                    <h3>Accurate Prayer Times</h3>
                    <p>Powered by the Adhan library with 12 calculation methods used worldwide. Auto-detects the right method based on your location.</p>
                </div>
                <div class="feature-card fade-in">
                    <div class="feature-icon pink">&#128276;</div>
                    <h3>Smart Notifications</h3>
                    <p>Get notified when a prayer starts and set reminders before it ends. Configurable warnings so you never miss a prayer.</p>
                </div>
                <div class="feature-card fade-in">
                    <div class="feature-icon indigo">&#9745;</div>
                    <h3>Prayer Tracking</h3>
                    <p>Mark prayers as completed with a simple tap. Resets daily at Fajr. A small thing that builds a big habit.</p>
                </div>
                <div class="feature-card fade-in">
                    <div class="feature-icon green">&#127759;</div>
                    <h3>Location Aware</h3>
                    <p>Uses your GPS location with reverse geocoding. Automatically updates prayer times when you travel. No manual city selection needed.</p>
                </div>
                <div class="feature-card fade-in">
                    <div class="feature-icon orange">&#128332;</div>
                    <h3>Jumu'ah Support</h3>
                    <p>Special handling for Friday prayers with configurable duration and optional fixed start times for your local mosque.</p>
                </div>
                <div class="feature-card fade-in">
                    <div class="feature-icon amber">&#129517;</div>
                    <h3>Qibla Compass</h3>
                    <p>Find the direction of the Ka'aba from wherever you are. A beautiful compass that shows you exactly which way to face for prayer.</p>
                </div>
            </div>
        </div>
    </div>
</section>
```

Also add this CSS for the amber feature icon color (in the CSS section near the other `.feature-icon` color rules):

```css
.feature-icon.amber { background: rgba(245, 166, 35, 0.15); }
```

- [ ] **Step 2: Verify features swap per tab**

Open in browser. macOS tab shows 6 cards + menu bar screenshot. Switch to mobile: 7 cards (Qibla added) + mobile prayer times screenshot.

- [ ] **Step 3: Commit**

```bash
git add athan/index.html
git commit -m "feat: add dual-platform features grid with Qibla card for mobile"
```

---

### Task 7: Add dual-platform Calendar USP section

The USP section swaps screenshots per tab. Copy is platform-neutral.

**Files:**
- Modify: `athan/index.html` (USP section)

- [ ] **Step 1: Replace USP section with dual-platform content**

Replace the existing USP section (`<section class="usp" id="calendar">...</section>`) with:

```html
<!-- USP: Calendar Integration -->
<section class="usp" id="calendar">
    <div class="container">
        <div class="usp-layout">
            <div class="usp-content fade-in">
                <div class="section-label">What makes Athan different</div>
                <h2 class="section-title">Never miss a prayer, even on your busiest days</h2>
                <p class="section-subtitle">Life happens. Meetings shift, days get packed, and prayer times can slip through the cracks. Athan's booking wizard reserves prayer slots as early as possible within each prayer window, working around your existing commitments — weeks or even months ahead of time.</p>

                <ul class="usp-features">
                    <li>
                        <span class="check">&#10003;</span>
                        <span>Auto-schedules prayers at the earliest available time in each prayer window</span>
                    </li>
                    <li>
                        <span class="check">&#10003;</span>
                        <span>Reads your calendar to avoid conflicts with existing commitments</span>
                    </li>
                    <li>
                        <span class="check">&#10003;</span>
                        <span>Plan ahead — book prayer times weeks or months in advance</span>
                    </li>
                    <li>
                        <span class="check">&#10003;</span>
                        <span>Adjust any proposed slot to fit your schedule</span>
                    </li>
                    <li>
                        <span class="check">&#10003;</span>
                        <span>Books directly into your calendar with one click</span>
                    </li>
                    <li>
                        <span class="check">&#10003;</span>
                        <span>Extended Jumu'ah duration with custom start times for Fridays</span>
                    </li>
                </ul>
            </div>

            <div class="fade-in" data-platform="macos">
                <img src="screenshot-book-prayers.png" alt="Athan calendar booking view showing prayer slots scheduled around meetings" class="screenshot">
            </div>

            <div class="fade-in" data-platform="mobile">
                <img src="screen_book_prayers.png" alt="Athan mobile calendar booking view" class="screenshot-mobile" style="max-width: 300px;">
            </div>
        </div>
    </div>
</section>
```

- [ ] **Step 2: Verify screenshot swaps per tab**

Open in browser. macOS shows the desktop booking screenshot. Mobile shows the mobile booking screenshot. Text stays the same.

- [ ] **Step 3: Commit**

```bash
git add athan/index.html
git commit -m "feat: add dual-platform calendar USP with swapping screenshots"
```

---

### Task 8: Add dual-platform How It Works section

Swaps step copy per tab while keeping the same 3-step structure.

**Files:**
- Modify: `athan/index.html` (How It Works section)

- [ ] **Step 1: Replace How It Works section with dual-platform content**

Replace the existing How It Works section with:

```html
<!-- How It Works -->
<section class="how-it-works">
    <div class="container">
        <div class="section-header fade-in">
            <div class="section-label">Getting started</div>
            <h2 class="section-title">Up and running in under a minute</h2>
        </div>

        <div class="steps" data-platform="macos">
            <div class="step fade-in">
                <div class="step-number">1</div>
                <h3>Install &amp; allow location</h3>
                <p>Download from the Mac App Store. The guided onboarding asks for location access to calculate accurate prayer times for where you are.</p>
            </div>
            <div class="step fade-in">
                <div class="step-number">2</div>
                <h3>Pick your method</h3>
                <p>Athan auto-detects the right calculation method for your region. Adjust the madhab and display preferences to match your practice.</p>
            </div>
            <div class="step fade-in">
                <div class="step-number">3</div>
                <h3>Pray on time</h3>
                <p>Prayer times appear in your menu bar. Get notified, track your prayers, and optionally book them into your calendar.</p>
            </div>
        </div>

        <div class="steps" data-platform="mobile">
            <div class="step fade-in">
                <div class="step-number">1</div>
                <h3>Install &amp; allow location</h3>
                <p>Download from the App Store or Google Play. The guided onboarding asks for location access to calculate accurate prayer times for where you are.</p>
            </div>
            <div class="step fade-in">
                <div class="step-number">2</div>
                <h3>Pick your method</h3>
                <p>Athan auto-detects the right calculation method for your region. Adjust the madhab and display preferences to match your practice.</p>
            </div>
            <div class="step fade-in">
                <div class="step-number">3</div>
                <h3>Pray on time</h3>
                <p>Prayer times are always a tap away on your home screen. Get notified, track your prayers, and optionally book them into your calendar.</p>
            </div>
        </div>
    </div>
</section>
```

- [ ] **Step 2: Verify steps swap per tab**

Open in browser and toggle between platforms. macOS mentions "menu bar" and "Mac App Store". Mobile mentions "home screen" and "App Store or Google Play".

- [ ] **Step 3: Commit**

```bash
git add athan/index.html
git commit -m "feat: add dual-platform How It Works steps"
```

---

### Task 9: Add dual-platform Settings section

Swaps settings screenshots per tab.

**Files:**
- Modify: `athan/index.html` (Settings section)

- [ ] **Step 1: Replace Settings section with dual-platform content**

Replace the existing settings section (`<section class="features" id="settings">...</section>`) with:

```html
<!-- Settings -->
<section class="features" id="settings">
    <div class="container">
        <div class="section-header fade-in">
            <div class="section-label">Fully configurable</div>
            <h2 class="section-title">Your practice, your preferences</h2>
            <p class="section-subtitle">Choose your calculation method, madhab, display mode, notifications, and Jumu'ah settings. Everything adapts to how you pray.</p>
        </div>

        <div class="settings-screenshots fade-in" data-platform="macos">
            <div>
                <img src="screenshot-settings-calculation.png" alt="Athan calculation settings" class="screenshot">
                <p class="screenshot-caption">Calculation method, madhab &amp; Jumu'ah settings</p>
            </div>
            <div>
                <img src="screenshot-settings-display.png" alt="Athan display settings" class="screenshot">
                <p class="screenshot-caption">Display mode, notifications &amp; reminders</p>
            </div>
        </div>

        <div class="settings-screenshots fade-in" data-platform="mobile">
            <div>
                <img src="screen_settings.png" alt="Athan mobile settings — location, calculation, Jumu'ah" class="screenshot-mobile" style="max-width: 100%;">
                <p class="screenshot-caption">Location, calculation method &amp; Jumu'ah settings</p>
            </div>
            <div>
                <img src="screen_settings2.png" alt="Athan mobile settings — calendar, display, notifications" class="screenshot-mobile" style="max-width: 100%;">
                <p class="screenshot-caption">Calendar, display &amp; notification settings</p>
            </div>
        </div>
    </div>
</section>
```

- [ ] **Step 2: Verify settings screenshots swap per tab**

macOS shows desktop settings. Mobile shows the two mobile settings screenshots.

- [ ] **Step 3: Commit**

```bash
git add athan/index.html
git commit -m "feat: add dual-platform settings screenshots"
```

---

### Task 10: Add dual-platform Pricing buttons and CTA/Download section

Pricing section is shared but download buttons swap per tab. CTA section fully swaps.

**Files:**
- Modify: `athan/index.html` (Pricing and CTA sections)

- [ ] **Step 1: Update pricing section download buttons**

In the pricing section, replace the two download buttons at the bottom of the pricing cards with tab-aware versions. Replace the existing pricing cards section with:

```html
<!-- Pricing -->
<section class="pricing" id="pricing">
    <div class="container">
        <div class="section-header fade-in">
            <div class="section-label">Pricing</div>
            <h2 class="section-title">Free to use. Pay once to unlock more.</h2>
            <p class="section-subtitle">Core prayer times and notifications are completely free. Calendar booking is a one-time purchase.</p>
        </div>

        <!-- macOS pricing buttons -->
        <div class="pricing-cards fade-in" data-platform="macos">
            <div class="pricing-card">
                <h3>Free</h3>
                <div class="price">$0</div>
                <div class="price-note">Free forever</div>
                <ul class="pricing-features">
                    <li><span class="icon">&#10003;</span> Accurate prayer times</li>
                    <li><span class="icon">&#10003;</span> Menu bar display (4 modes)</li>
                    <li><span class="icon">&#10003;</span> Smart notifications</li>
                    <li><span class="icon">&#10003;</span> Prayer tracking</li>
                    <li><span class="icon">&#10003;</span> 12 calculation methods</li>
                    <li><span class="icon">&#10003;</span> Sunrise, sunset &amp; sunnah times</li>
                    <li><span class="icon disabled">&#8212;</span><span style="color: var(--text-dim)">Calendar booking</span></li>
                </ul>
                <a href="https://apps.apple.com/us/app/athan-for-mac/id6760509284" class="btn-secondary" style="width: 100%; justify-content: center;">Download free</a>
            </div>

            <div class="pricing-card featured">
                <h3>Calendar Booking</h3>
                <div class="price">$4.99 <span>one-time</span></div>
                <div class="price-note">No subscription. Yours forever.</div>
                <ul class="pricing-features">
                    <li><span class="icon">&#10003;</span> Everything in Free</li>
                    <li><span class="icon">&#10003;</span> Smart calendar scheduling</li>
                    <li><span class="icon">&#10003;</span> Drag-and-drop slot editing</li>
                    <li><span class="icon">&#10003;</span> Conflict detection</li>
                    <li><span class="icon">&#10003;</span> Jumu'ah booking</li>
                    <li><span class="icon">&#10003;</span> Week-at-a-glance view</li>
                    <li><span class="icon">&#10003;</span> One-click booking</li>
                </ul>
                <a href="https://apps.apple.com/us/app/athan-for-mac/id6760509284" class="btn-primary" style="width: 100%; justify-content: center;">Get Athan</a>
            </div>
        </div>

        <!-- Mobile pricing buttons -->
        <div class="pricing-cards fade-in" data-platform="mobile">
            <div class="pricing-card">
                <h3>Free</h3>
                <div class="price">$0</div>
                <div class="price-note">Free forever</div>
                <ul class="pricing-features">
                    <li><span class="icon">&#10003;</span> Accurate prayer times</li>
                    <li><span class="icon">&#10003;</span> Smart notifications</li>
                    <li><span class="icon">&#10003;</span> Prayer tracking</li>
                    <li><span class="icon">&#10003;</span> 12 calculation methods</li>
                    <li><span class="icon">&#10003;</span> Qibla compass</li>
                    <li><span class="icon">&#10003;</span> Sunrise, sunset &amp; sunnah times</li>
                    <li><span class="icon disabled">&#8212;</span><span style="color: var(--text-dim)">Calendar booking</span></li>
                </ul>
                <a href="#download" class="btn-secondary" style="width: 100%; justify-content: center;">Download free</a>
            </div>

            <div class="pricing-card featured">
                <h3>Calendar Booking</h3>
                <div class="price">$4.99 <span>one-time</span></div>
                <div class="price-note">No subscription. Yours forever.</div>
                <ul class="pricing-features">
                    <li><span class="icon">&#10003;</span> Everything in Free</li>
                    <li><span class="icon">&#10003;</span> Smart calendar scheduling</li>
                    <li><span class="icon">&#10003;</span> Conflict detection</li>
                    <li><span class="icon">&#10003;</span> Jumu'ah booking</li>
                    <li><span class="icon">&#10003;</span> Week-at-a-glance view</li>
                    <li><span class="icon">&#10003;</span> One-click booking</li>
                </ul>
                <a href="#download" class="btn-primary" style="width: 100%; justify-content: center;">Get Athan</a>
            </div>
        </div>
    </div>
</section>
```

Note: macOS pricing retains "Menu bar display (4 modes)" and "Drag-and-drop slot editing". Mobile pricing adds "Qibla compass" to free tier and omits drag-and-drop. macOS pricing buttons link to Mac App Store; mobile pricing buttons link to `#download` section (where platform-specific store links live).

- [ ] **Step 2: Replace CTA/Download section with dual-platform content**

Replace the existing CTA section with:

```html
<!-- CTA -->
<section class="cta" id="download">
    <div class="container fade-in" data-platform="macos">
        <div class="section-label">Download</div>
        <h2 class="section-title">Start praying on time today</h2>
        <p class="section-subtitle">Athan is free to download. Install it once and it quietly keeps you on track, every single day.</p>
        <a href="https://apps.apple.com/us/app/athan-for-mac/id6760509284" class="btn-primary" style="font-size: 1.15rem; padding: 1rem 2.5rem;">
            <svg width="22" height="22" viewBox="0 0 20 20" fill="currentColor"><path d="M15.6 8.4c-.1-2.3 1.9-3.4 2-3.5-1.1-1.6-2.8-1.8-3.4-1.8-1.4-.1-2.8.9-3.5.9-.7 0-1.8-.8-3-.8-1.5 0-2.9.9-3.7 2.3-1.6 2.8-.4 6.9 1.1 9.1.8 1.1 1.7 2.3 2.9 2.3 1.1 0 1.6-.7 2.9-.7 1.4 0 1.8.7 3 .7 1.2 0 2-1.1 2.8-2.2.9-1.3 1.2-2.5 1.3-2.6-.1-.1-2.4-1-2.4-3.7zM13.4 2.8c.6-.8 1-1.8.9-2.8-.9 0-2 .6-2.6 1.4-.5.7-1 1.8-.9 2.8 1 .1 2-.5 2.6-1.4z"/></svg>
            Download on the Mac App Store
        </a>
        <div class="requirements-badge">Requires macOS 14 Sonoma or later</div>
    </div>

    <div class="container fade-in" data-platform="mobile">
        <div class="section-label">Download</div>
        <h2 class="section-title">Start praying on time today</h2>
        <p class="section-subtitle">Athan is free to download. Install it once and it quietly keeps you on track, every single day.</p>
        <div class="hero-buttons" style="justify-content: center; margin-bottom: 1rem;">
            <a href="#" class="btn-primary" style="font-size: 1.15rem; padding: 1rem 2.5rem;">
                <svg width="22" height="22" viewBox="0 0 20 20" fill="currentColor"><path d="M15.6 8.4c-.1-2.3 1.9-3.4 2-3.5-1.1-1.6-2.8-1.8-3.4-1.8-1.4-.1-2.8.9-3.5.9-.7 0-1.8-.8-3-.8-1.5 0-2.9.9-3.7 2.3-1.6 2.8-.4 6.9 1.1 9.1.8 1.1 1.7 2.3 2.9 2.3 1.1 0 1.6-.7 2.9-.7 1.4 0 1.8.7 3 .7 1.2 0 2-1.1 2.8-2.2.9-1.3 1.2-2.5 1.3-2.6-.1-.1-2.4-1-2.4-3.7zM13.4 2.8c.6-.8 1-1.8.9-2.8-.9 0-2 .6-2.6 1.4-.5.7-1 1.8-.9 2.8 1 .1 2-.5 2.6-1.4z"/></svg>
                Download on the App Store
            </a>
            <a href="#" class="btn-play" style="font-size: 1.15rem; padding: 1rem 2.5rem;">
                <svg width="22" height="22" viewBox="0 0 24 24" fill="currentColor"><path d="M3.609 1.814L13.792 12 3.61 22.186a.996.996 0 0 1-.61-.92V2.734a1 1 0 0 1 .609-.92zm10.89 10.893l2.302 2.302-10.937 6.333 8.635-8.635zm3.199-3.199l2.302 2.302a1 1 0 0 1 0 1.38l-2.302 2.302L15.395 12l2.303-2.492zM5.864 2.658L16.8 8.99l-2.302 2.302L5.864 2.658z"/></svg>
                Get it on Google Play
            </a>
        </div>
        <div class="requirements-badge">Requires iOS 17 or later / Android 10 or later</div>
    </div>
</section>
```

Note: App Store and Google Play URLs use `#` as placeholders — replace with actual URLs when available.

- [ ] **Step 3: Verify pricing and CTA swap correctly**

Open in browser. Pricing section should look the same for both tabs (platform-neutral). CTA section should show Mac App Store button for macOS tab, and App Store + Google Play buttons for mobile tab.

- [ ] **Step 4: Commit**

```bash
git add athan/index.html
git commit -m "feat: add platform-neutral pricing and dual-platform CTA section"
```

---

### Task 11: Update privacy policy to be platform-neutral

**Files:**
- Modify: `athan/privacy.html`

- [ ] **Step 1: Update privacy policy content**

Make the following replacements in `athan/privacy.html`:

1. Title: keep as "Privacy Policy — Athan"
2. Highlight box (line 164): replace "Athan does not collect, store, or transmit any personal data. Everything stays on your Mac." with "Athan does not collect, store, or transmit any personal data. Everything stays on your device."
3. Overview (line 168): replace "Athan is a macOS menu bar application for Islamic prayer times. It is designed with privacy as a core principle. The app operates entirely on your device and does not communicate with any external servers." with "Athan is an application for macOS, iOS, and Android for Islamic prayer times. It is designed with privacy as a core principle. The app operates entirely on your device and does not communicate with any external servers."
4. Location Data (line 173): replace "Processed entirely on your device using the macOS Core Location framework" with "Processed entirely on your device using your device's location services"
5. Location Data (line 178): replace "You can deny location access at any time in System Settings. The app will continue to function if you have previously allowed location access, using the last known coordinates." with "You can deny location access at any time in your device's settings. The app will continue to function if you have previously allowed location access, using the last known coordinates."
6. Calendar Data (line 181): replace "If you use the optional calendar booking feature, Athan requests access to your macOS Calendar. This data is:" with "If you use the optional calendar booking feature, Athan requests access to your device's calendar. This data is:"
7. Calendar Data (line 185): replace "Never transmitted, copied, or shared outside the macOS Calendar framework" with "Never transmitted, copied, or shared outside your device's calendar framework"
8. Calendar Data (line 187): replace "Calendar access is optional and only requested when you open the booking feature. You can revoke it at any time in System Settings." with "Calendar access is optional and only requested when you open the booking feature. You can revoke it at any time in your device's settings."
9. Notifications (line 190): replace "Athan uses the macOS notification system to alert you when prayers start or are about to end. Notification content is generated locally and is not sent to any server." with "Athan uses your device's notification system to alert you when prayers start or are about to end. Notification content is generated locally and is not sent to any server."
10. Prayer Tracking (line 193): replace "This tracking data is stored locally on your device using macOS UserDefaults. It is never transmitted, synced, or shared. The data resets daily at Fajr time." with "This tracking data is stored locally on your device. It is never transmitted, synced, or shared. The data resets daily at Fajr time."
11. Preferences (line 196): replace "Your settings (calculation method, madhab, display preferences) are stored locally using macOS UserDefaults. This data:" with "Your settings (calculation method, madhab, display preferences) are stored locally on your device. This data:"
12. Adhan library (line 210): update the link text only if the mobile version uses a different library (e.g., `adhan-java`/`adhan-kotlin` for Android). If same, keep as-is.
13. Analytics & Tracking (line 207): verify whether the "zero network requests" claim holds for the mobile apps. If the mobile app makes any network requests (e.g., for Google Play billing, remote config, etc.), adjust the wording accordingly (e.g., "The app does not make network requests to collect or transmit personal data.").

- [ ] **Step 2: Verify privacy page renders correctly**

Open `athan/privacy.html` in a browser. All text should be platform-neutral with no "macOS" references remaining (except in the overview where it's listed as one of the platforms).

- [ ] **Step 3: Commit**

```bash
git add athan/privacy.html
git commit -m "docs: update privacy policy to be platform-neutral for macOS, iOS, and Android"
```

---

### Task 12: Update main site index.html

**Files:**
- Modify: `index.html` (Tools section)

- [ ] **Step 1: Update Athan card platform label**

In `index.html`, find the Athan tool card and change the platform span from:

```html
<span class="tool-platform">macOS</span>
```

to:

```html
<span class="tool-platform">macOS · iOS · Android</span>
```

- [ ] **Step 2: Verify main page shows updated platform label**

Open `index.html` in browser. The Athan card should show "macOS · iOS · Android".

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: update Athan card to show all platforms"
```

---

### Task 13: Final verification and polish

**Files:**
- Review: `athan/index.html`, `athan/privacy.html`, `index.html`

- [ ] **Step 1: Test full tab switching flow**

Open `athan/index.html` in browser and verify:
1. Default loads macOS tab
2. Clicking "iOS & Android" toggle switches all sections to mobile content
3. URL updates to `?platform=mobile`
4. Browser back button returns to macOS tab
5. Direct URL `?platform=mobile` loads mobile tab on page load
6. Combined URL `?platform=mobile#features` loads mobile tab and scrolls to features
7. Arrow keys switch between tabs when toggle is focused
8. All in-page anchor links (`#features`, `#pricing`, etc.) still scroll correctly on both tabs

- [ ] **Step 2: Test responsive layout**

Resize browser to mobile width (< 600px) and verify:
1. Platform toggle remains visible in nav
2. Nav links are hidden (as before)
3. All sections stack correctly
4. Mobile screenshots display at appropriate sizes
5. Two download buttons in mobile CTA stack or wrap gracefully

- [ ] **Step 3: Cross-link verification**

1. From `index.html`, click Athan card → should go to `/athan/`
2. From `/athan/`, click privacy link → should go to `privacy.html`
3. From `privacy.html`, click back → should return to `/athan/`

- [ ] **Step 4: Final commit if any polish needed**

```bash
git add -A
git commit -m "polish: final adjustments after testing"
```
