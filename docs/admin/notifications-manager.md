# Notification Manager & Smart Title Seeder

The **Notification Manager** powers viewer retention and engagement by delivering real-time in-app alerts, release notifications, and broadcast messages to users on both desktop browsers and mobile PWAs. Located at **iFlix Stream** -> **Notifications** in `wp-admin`, it allows administrators to craft custom promotional messages or trigger automated smart notifications based on fresh releases.

---

## 🔔 Delivery Channels

1. **Header Notification Bell**: Displays a pulsing red badge when unread alerts exist. Clicking reveals a dropdown drawer with media thumbnails and direct "Watch Now" links.
2. **Mobile Notifications Hub** (`/notifications/`): A full-width mobile view dedicated to browsing past release announcements.
3. **PWA Push Notifications**: Optional Web Push integration delivering alerts even when the browser or app is closed.

---

## ✍️ Creating Broadcast Notifications

To send an announcement to all platform viewers:
1. In `wp-admin`, navigate to **iFlix Stream** -> **Notifications**.
2. Click **+ Create Notification**.
3. Fill in the message details:
   - **Notification Title**: Catchy headline (e.g., `Season 2 is Now Streaming!`).
   - **Message Body**: Short synopsis or announcement text (e.g., `The wait is over. All 10 episodes are now available in 4K Ultra HD.`).
   - **Target Link / URL**: Deep link to a specific movie or episode (e.g., `/watch/1399/` or `/subscription/`).
   - **Media Thumbnail**: Optional image URL (or automatic TMDB poster link).
   - **Badge / Category**: `New Release`, `Announcement`, `Subscription Alert`, or `Maintenance`.
4. Click **Publish Notification**.

---

## ⚡ Smart Title Seeder (1-Click Automated Ingestion)

Content managers do not need to manually type notifications for every trending movie. iFlix Stream includes an automated seeder:

1. Click **Seed Smart Notifications** in the Notification Manager.
2. The system executes `IFLIX\Core\Admin\Dashboard::ajax_seed_smart_notifications()`.
3. The engine contacts TMDB, pulls the top 5 trending releases today, and automatically formats individual notification items complete with title, backdrop, synopsis snippet, and direct watch links.
