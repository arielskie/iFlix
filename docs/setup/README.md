# Setup & Deployment Guides

Welcome to the **Setup & Deployment** guide series for **iFlix Stream**. This section covers initial server preparation, installation of the WordPress theme and core plugin, API authentication, cloud database provisioning, and production deployment checklists.

---

## 📋 Guides in this Section

| Document | Purpose | Time Required |
|---|---|---|
| [**installation.md**](installation.md) | WordPress theme and plugin setup, rewrite rule verification, and web server configuration. | ~5 minutes |
| [**tmdb-configuration.md**](tmdb-configuration.md) | Obtaining and configuring TMDB v3 API credentials, cache controls, and localized catalogs. | ~3 minutes |
| [**firebase-configuration.md**](firebase-configuration.md) | Provisioning Google Firebase Auth and Firestore database for cross-device viewer sync. | ~8 minutes |
| [**troubleshooting-faq.md**](troubleshooting-faq.md) | Resolving common installation errors, CORS playback warnings, and rewrite rule issues. | Reference |

---

## ⚡ Deployment Prerequisites Checklist

Before proceeding with the installation, verify that your hosting environment meets the following specifications:

- [ ] **PHP Version**: PHP 8.1 or PHP 8.2 (with `opcache`, `curl`, `json`, `mbstring`, `openssl`, and `gd` enabled).
- [ ] **WordPress**: Version 6.0 or higher (fresh install or clean site recommended).
- [ ] **Web Server**: Apache with `mod_rewrite` enabled OR Nginx with standard WordPress `try_files` directive.
- [ ] **SSL / HTTPS**: Valid SSL certificate active (Let's Encrypt, Cloudflare SSL, or commercial certificate). HTTPS is required for Service Workers and PWA capabilities.
- [ ] **TMDB Developer Account**: Free account on [themoviedb.org](https://www.themoviedb.org/) for metadata and media ingestion.
- [ ] **Google Firebase Project**: Free account on [console.firebase.google.com](https://console.firebase.google.com/) for authentication and watchlist synchronization.
