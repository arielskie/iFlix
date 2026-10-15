# Troubleshooting & Frequently Asked Questions (FAQ)

This guide covers diagnostic procedures, common error codes, and step-by-step solutions for issues encountered during deployment or maintenance of **iFlix Stream**.

---

## 🔍 Diagnostic Checklist

When troubleshooting any issue:
1. Open your browser Developer Tools (**F12** or **Cmd + Option + I**) and check the **Console** tab for red JavaScript errors.
2. Check your WordPress PHP error log (typically located at `wp-content/debug.log` if `WP_DEBUG_LOG` is enabled in `wp-config.php`).
3. Verify that your TMDB API Key status is green in **iFlix Stream** -> **Settings** -> **TMDB API**.

---

## ❓ Frequently Asked Questions

### 1. Watch URLs or Movie Pages return a 404 Not Found error
- **Cause**: WordPress rewrite rules have not been flushed after activating the theme.
- **Solution**:
  1. Go to **Settings** -> **Permalinks** in your WordPress dashboard.
  2. Ensure **Post name** is selected.
  3. Click **Save Changes**. This forces WordPress to regenerate `.htaccess` and register `watch`, `movie`, `tv`, and `subscription` endpoints.

---

### 2. TMDB Titles, Images, or Sliders are not loading
- **Cause A: Invalid or Unactivated TMDB Key**:
  - Visit **iFlix Stream** -> **Settings** -> **TMDB API** and click **Test TMDB Connection**. If it fails with `401 Unauthorized`, verify your API key string.
- **Cause B: Server Outbound HTTP Blocks**:
  - Some budget shared hosts block outbound cURL connections to external APIs. Test server outbound requests via WP-CLI:
    ```bash
    wp eval 'var_dump(wp_remote_get("https://api.themoviedb.org/3/movie/550?api_key=YOUR_KEY"));'
    ```
- **Cause C: Stale Transient Cache**:
  - In **iFlix Stream** -> **Dashboard & Health**, click **Flush TMDB Cache** to purge old API responses.

---

### 3. Video Player displays "Video Stream Offline" or CORS Error
- **Cause A: Cross-Origin Resource Sharing (CORS)**:
  - If loading direct `.m3u8` or `.mp4` files from your own CDN or storage bucket, the storage server must return the appropriate CORS header:
    ```http
    Access-Control-Allow-Origin: *
    Access-Control-Allow-Methods: GET, HEAD, OPTIONS
    ```
- **Cause B: HTTPS / Mixed Content Warning**:
  - If your WordPress site runs on `https://` and your video stream is served over insecure `http://`, modern browsers will block playback. Ensure all stream URLs use `https://`.
- **Cause C: Iframe Embed Sandbox Restrictions**:
  - If an embed requires top-level redirection to function, it will be blocked by default for security. You can adjust sandbox settings in **iFlix Stream** -> **Settings** -> **Player Settings**.

---

### 4. "+ Add to My List" button doesn't save items
- **Cause A: Unsubscribed User Gating**:
  - If monetization paywalls are enabled in **Settings** -> **Subscription**, users must hold an active subscription tier to save titles to their watchlist. Non-subscribers will be prompted with the "Subscription Required" modal.
- **Cause B: Firebase Not Configured**:
  - If the user is logged in, ensure Firebase credentials in **Settings** -> **Firebase** are valid and that Firestore security rules have been published.

---

### 5. PWA "Install App" prompt is not appearing
- **Cause A: Insecure Connection**:
  - Progressive Web Apps strictly require a valid SSL certificate (`https://`). PWA will not install over plain `http://` (except on `localhost`).
- **Cause B: Missing Icons**:
  - Ensure `icon-192.png` and `icon-512.png` exist in `wp-content/themes/iflix/assets/images/`.
- **Cause C: Browser Already Installed**:
  - If the app is already installed or previously dismissed, Chrome suppresses the banner for a cooldown period. Test in an Incognito window.

---

### 6. CinePro Streams return "403 Forbidden" or "API Key Invalid"
- **Cause**: The API key configured in **Settings** -> **CinePro Integration** does not match the master key on your self-hosted CinePro instance.
- **Solution**: Check your CinePro server `.env` file for `CINEPRO_API_KEY` and update the value in WordPress accordingly.
