# Installation & Server Setup Guide

This guide provides step-by-step instructions for deploying the **iFlix Stream Theme** and **iFlix Stream Core Plugin** onto a production or local WordPress server.

---

## 🏗️ 1. Server Architecture & PHP Configuration

Ensure your PHP configuration (`php.ini`) meets the recommended production thresholds:

```ini
; Recommended Production php.ini settings for iFlix Stream
memory_limit = 256M
upload_max_filesize = 64M
post_max_size = 64M
max_execution_time = 180
max_input_time = 180
max_input_vars = 3000

; Enable necessary extensions
extension=curl
extension=openssl
extension=mbstring
extension=gd
```

---

## 📦 2. Theme & Plugin Installation

The platform consists of two synchronized packages:
1. **iFlix Stream Theme** (`wp-content/themes/iflix/`): Contains the Netflix-inspired dark streaming layout, interactive carousels, responsive modals, PWA manifest, and frontend styling.
2. **iFlix Stream Core Plugin** (`wp-content/plugins/iflix-core/`): Contains the TMDB API engine, transient caching layer, CinePro stream bridge, Video Source CPT, and WordPress admin settings.

### Method A: WordPress Admin Upload (Recommended)
1. In your WordPress dashboard, navigate to **Appearance** -> **Themes** -> **Add New** -> **Upload Theme**.
2. Select `iflix.zip` and click **Install Now**. Once uploaded, click **Activate**.
3. Next, navigate to **Plugins** -> **Add New** -> **Upload Plugin**.
4. Select `iflix-core.zip` and click **Install Now**. Once uploaded, click **Activate Plugin**.

### Method B: Manual Git / SFTP Deployment
```bash
# Upload theme to your themes folder
cd /var/www/html/wp-content/themes/
git clone https://github.com/yourusername/iflix-stream.git iflix

# Upload core plugin to plugins folder
cd /var/www/html/wp-content/plugins/
git clone https://github.com/yourusername/iflix-stream-core.git iflix-core
```
Then activate both components via **WP-CLI**:
```bash
wp theme activate iflix
wp plugin activate iflix-core
```

---

## 🔀 3. Permalink Structure & Custom Rewrite Rules

iFlix Stream relies on clean, Netflix-style routing for movie and episode watch pages:
- `/watch/{tmdb_id}/` (Movies)
- `/watch/{tmdb_id}/season-{season_number}/episode-{episode_number}/` (TV Episodes)
- `/movie/{tmdb_id}/` (Movie Details)
- `/tv/{tmdb_id}/` (TV Show Details)
- `/subscription/` (Monetization & Plans)
- `/my-list/`, `/history/`, `/profile/`, `/account/` (User Hubs)

### Re-saving Permalinks
1. Navigate to **Settings** -> **Permalinks** in your WordPress dashboard.
2. Select **Post name** (`/%postname%/`).
3. Click **Save Changes** at the bottom of the page. This automatically flushes rewrite rules and compiles the theme's custom query vars (`iflix_id`, `iflix_season`, `iflix_episode`, `iflix_type`).

---

## 🌐 4. Web Server Configuration

### Nginx Configuration
If you run Nginx, ensure your `server` block routes non-file requests to WordPress `index.php`:

```nginx
server {
    listen 443 ssl http2;
    server_name streaming.yourdomain.com;
    root /var/www/html;
    index index.php index.html;

    # Gzip Compression for Fast Assets
    gzip on;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml text/javascript;

    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.2-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    # Browser Caching for Media and Fonts
    location ~* \.(jpg|jpeg|png|gif|svg|webp|woff|woff2|ttf|eot)$ {
        expires 30d;
        add_header Cache-Control "public, no-transform";
    }

    # Service Worker and PWA Manifest (Must not be aggressively cached)
    location ~* (service-worker\.js|manifest\.json)$ {
        expires -1;
        add_header Cache-Control "no-cache, no-store, must-revalidate";
    }
}
```

### Apache `.htaccess` Configuration
Ensure your root `.htaccess` file has `mod_rewrite` enabled:

```apache
# BEGIN WordPress
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]
</IfModule>
# END WordPress
```

---

## ✅ 5. Verification Checklist

1. Visit your home URL: `https://streaming.yourdomain.com/` — You should see the cinematic Netflix dark landing page or guest billboard.
2. Navigate to `/subscription/` — Verify that the plan matrix loads with interactive cards.
3. Access `/wp-admin/` and look for the **iFlix Stream** menu icon in the sidebar.
