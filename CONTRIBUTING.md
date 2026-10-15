# Contributing to iFlix Stream

Thank you for your interest in contributing to **iFlix Stream**! We welcome bug reports, feature suggestions, architectural improvements, and code contributions from the open-source community.

---

## 📜 Code of Conduct

By participating in this project, you agree to maintain a respectful, inclusive, and professional environment for all contributors and community members.

---

## 🛠️ Development Setup & Workflow

### 1. Prerequisites
- **Local WordPress Environment**: LocalWP, XAMPP, Docker, or Valet running WordPress 6.0+ on PHP 8.1+.
- **Node.js**: v18+ (if compiling or linting assets).
- **Git**: Installed and configured on your machine.
- **TMDB API Key**: Free developer key from [themoviedb.org](https://www.themoviedb.org/).
- **Firebase Project**: Free tier project on [Firebase Console](https://console.firebase.google.com/).

### 2. Fork and Clone
```bash
# Clone the theme
git clone https://github.com/yourusername/iflix-stream.git wp-content/themes/iflix

# Clone the core engine plugin
git clone https://github.com/yourusername/iflix-stream-core.git wp-content/plugins/iflix-core
```

### 3. Branching Strategy
- Always create a descriptive feature branch from `master` or `main`:
  ```bash
  git checkout -b feature/hls-quality-selector
  # or
  git checkout -b fix/mobile-carousel-touch
  ```

---

## 🎨 Coding Standards

### PHP (WordPress Coding Standards)
- Follow standard [WordPress Coding Standards (WPCS)](https://developer.wordpress.org/coding-standards/wordpress-coding-standards/php/).
- Always sanitize inputs (`sanitize_text_field`, `absint`, `esc_url_raw`) and escape outputs (`esc_html`, `esc_attr`, `esc_url`).
- Verify Nonces for all AJAX endpoints (`check_ajax_referer` or `wp_verify_nonce`).
- Utilize strict type checking and avoid global variables where namespaced singletons or static classes are available (`IFLIX\Core\...`).

### JavaScript (ES6+ Standards)
- Use standard ES6+ syntax (`const`, `let`, arrow functions, `async/await`).
- Do not introduce heavy dependencies or jQuery into customer-facing player components. Keep `iflix-player.js` modular and dependency-light.
- Handle DOM events with passive touch listeners where applicable to preserve 60fps scrolling performance on mobile devices.

### CSS / Styling
- Follow the Netflix Dark Theme palette variables (`--theme-accent: #E50914`, `--bg-dark: #141414`, `--text-muted: #808080`).
- Ensure all components are responsive across mobile (<768px), tablet (<1024px), desktop, and ultra-wide screens (>1440px).

---

## 🧪 Testing Your Changes

Before submitting your pull request:
1. Verify video playback across both direct HLS streams and iframe embed fallbacks.
2. Verify cross-browser compatibility (Chrome, Safari iOS, Firefox, Edge).
3. Test guest mode (`localStorage`) vs authenticated mode (`Firestore`) state transitions.
4. Check that no PHP warnings, notices, or JavaScript console errors are emitted.

---

## 🚀 Submitting a Pull Request (PR)

1. Commit your changes with clear, semantic commit messages:
   ```bash
   git commit -m "feat(player): add dual audio track switching"
   ```
2. Push to your fork:
   ```bash
   git push origin feature/your-feature-name
   ```
3. Open a Pull Request against the `master` branch.
4. Describe the problem your PR solves, screenshots or video clips of the behavior, and any breaking changes.
