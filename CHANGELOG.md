# Changelog

All notable changes to the **iFlix Stream** theme and **iFlix Stream Core** plugin will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [1.4.1] - 2026-09-23

### Fixed
- **Hero Billboard Add to List (`+`) Button**: Added missing `data-title`, `data-poster`, `data-backdrop`, `data-rating`, and `data-year` attributes to hero slider buttons in `front-page.php` and `functions.php`.
- **Dynamic DOM Title Fallback**: Added multi-container fallback title resolution in `toggleMyList()` in `app.js` to ensure titles with custom logos save smoothly without failing.
- **Paywall Watch Enforcement**: Corrected `$paywall_enabled` definition in `page-watch.php` ensuring subscriber paywall overlay gates playback for non-subscribers.
- **Mobile Plan Card Spacing**: Added 20px gap spacing between mobile subscription plan cards in `main.css`.
- **Cancel Plan Confirmation Modal**: Replaced basic browser dialog with a fullscreen mobile modal featuring top-left `< Back` button and danger action buttons.

---

## [1.4.0] - 2026-09-21

### Added
- **Multi-Profile PIN Lock**: Added 4-digit parental PIN security when switching away from Kids profiles to adult profiles.
- **Dedicated Kids Landing Mode**: Restricted content catalog filtering for G and PG titles only when a Kids profile is active.
- **Custom Profile Avatars**: Expanded avatar selector with 8 high-resolution themed SVG avatars.

### Changed
- **Profile Limit Allocation**: Synchronized maximum profiles per account based on subscription plan tier (Free: 1 profile, Basic: 2 profiles, Standard: 3 profiles, Premium: 6 profiles).

---

## [1.3.0] - 2026-09-19

### Added
- **Smart Notification System**: Added In-App notification bell with badge counter and slide-out notification drawer.
- **Automated Title Seeding**: 1-click seeder to generate notifications for recently released popular movies and trending series.
- **Mobile Notification View**: Dedicated responsive notification archive page (`/notifications/`).

### Changed
- **PWA Service Worker Optimization**: Improved asset caching strategy to cache posters and backdrops locally in IndexedDB/CacheStorage for faster repeat visits.

---

## [1.2.0] - 2026-09-17

### Added
- **Monetization & Paywall Engine**: Added complete subscription management supporting Free Guest Pass, Basic, Standard, and Premium 4K plans.
- **Resolution Tier Gating**: Enforced HLS resolution capping based on user subscription (360p, 720p, 1080p, 4K Ultra HD).
- **Interactive Checkout Modal**: Step-by-step subscription activation and upgrade flow with plan comparison matrix.

### Changed
- **User Settings & Account Hub**: Redesigned `/account/` page with active membership details, renewal dates, and payment history.

---

## [1.1.0] - 2026-09-15

### Added
- **CinePro Core Streaming Integration**: Native API bridge to self-hosted CinePro stream extractors for zero-ad HLS (`.m3u8`) video delivery.
- **Multi-Server Video Source CPT**: Custom Post Type (`iflix_video_source`) allowing administrators to configure multiple video streaming mirrors per title or globally.
- **Server Failover Engine**: Automatic fallback cascade in `iflix-player.js` when a primary server experiences timeouts or geo-restrictions.

---

## [1.0.0] - 2026-09-01

### Added
- **Initial Public Release of iFlix Stream Theme & Core Plugin**.
- Authentic 1:1 Netflix cinematic dark theme with billboard slider, responsive carousels, and hover video previews.
- TMDB API v3 automated synchronization for Movies and TV Series.
- Firebase Authentication (Email/Password & Google Sign-In) with Cloud Firestore sync for My List, History, and Continue Watching.
- iflixPlayer custom HTML5 video player with keyboard controls, skip intro, and auto-next episode.
- Section Builder and TMDB Endpoint Presets manager in WordPress Admin Studio.
