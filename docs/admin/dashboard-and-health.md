# Dashboard & System Health Monitor

The **Dashboard & Health** interface serves as the mission control center for your iFlix Stream platform. Located at **iFlix Stream** -> **Dashboard** in `wp-admin`, it provides live telemetry on API connectivity, database cache status, video sources, and system integrity.

---

## 📊 Live Metrics & Telemetry

### 1. TMDB API Connectivity Status
- **Status Indicator**: Live green badge (`Connected`) or red alert (`Unreachable / Invalid Key`).
- **Ping Latency**: Measures real-time round-trip latency to `api.themoviedb.org` via `wp_remote_get()`. Normal thresholds: `< 250ms`.
- **Transient Cache Records**: Displays the total count of active cached TMDB movie/TV titles currently residing in your WordPress database.

### 2. Video Streaming Pipeline
- **Active Video Sources**: Total count of active, published source definitions configured in the Video Sources CPT.
- **CinePro Status**: Reflects whether native direct HLS extraction is enabled and connected to your backend extractor.

### 3. Firebase & Cloud Sync Status
- **Project Linkage**: Shows whether Firebase Web credentials are fully provisioned.
- **Active Collections**: Reflects connected Firestore collections for profile synchronization.

---

## 🧹 Quick Maintenance Actions

### 1-Click Flush TMDB Cache
- **When to Use**: When you update content categories, add custom TMDB endpoints, or want newly released posters to replace cached artwork immediately.
- **How it Works**: Executes an asynchronous AJAX call (`wp_ajax_iflix_flush_cache`) that queries the `wp_options` table and deletes all transient keys matching `_transient_iflix_*` and `_transient_timeout_iflix_*`.
- **Safety**: Safe to run at any time; will never affect user accounts, watch history, or payment records.

---

## 🏥 Health Diagnostic Codes

| Diagnostic Check | Recommended State | Warning / Failure State | Action |
|---|---|---|---|
| **PHP Memory Limit** | `>= 256M` | `< 128M` | Increase `memory_limit` in `php.ini` |
| **cURL Version** | `>= 7.68.0` with OpenSSL | cURL disabled | Enable `php_curl` in server configuration |
| **HTTPS Status** | Active (TLS 1.2+) | Plain HTTP (`http://`) | Provision an SSL certificate to enable PWA and secure playback |
| **Image Engine** | GD 2.0+ or Imagick | Missing | Install `php-gd` or `php-imagick` |
