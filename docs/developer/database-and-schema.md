# Database Schema & Storage Model

This document outlines the data persistence model for **iFlix Stream**, covering WordPress core database tables (`wp_options`, `wp_usermeta`, `wp_postmeta`), Custom Post Types, and Google Cloud Firestore document hierarchies.

---

## 🗄️ 1. WordPress Options Schema (`wp_options`)

The core engine stores global configuration settings as serialized associative arrays:

| Option Key | Data Type | Structure / Contents |
|---|---|---|
| `iflix_tmdb_settings` | `array` | `api_key`, `language` (e.g. `en-US`), `include_adult` (bool), `cache_ttl` (int). |
| `iflix_firebase_settings` | `array` | `api_key`, `auth_domain`, `project_id`, `storage_bucket`, `messaging_sender_id`, `app_id`. |
| `iflix_player_settings` | `array` | `auto_play` (bool), `auto_next` (bool), `skip_intro_time` (int), `enable_iframe_sandbox` (bool). |
| `iflix_subscription_settings` | `array` | `enable_paywall` (bool), `currency_symbol` (str), `plan_basic_price`, `plan_standard_price`, `plan_premium_price`, `payment_mode`. |
| `iflix_cinepro_enabled` | `string` | `'1'` (enabled) or `'0'` (disabled). |
| `iflix_cinepro_api_url` | `string` | URL to CinePro server instance. |
| `iflix_cinepro_api_key` | `string` | Secret bearer authorization token. |
| `iflix_homepage_sections` | `array` | Array of section objects defining homepage row order, titles, card formats, and data sources. |
| `iflix_endpoint_presets` | `array` | Array of custom TMDB query filters and endpoint definitions. |

---

## 📄 2. Video Sources CPT Schema (`iflix_video_source`)

Stored in `wp_posts` with `post_type = 'iflix_video_source'`. Metadata stored in `wp_postmeta`:

| Post Meta Key | Data Type | Description |
|---|---|---|
| `_iflix_type` | `string` | `'movie'` or `'tv'`. |
| `_iflix_tmdb_id` | `int` | `0` (Global Default) or specific TMDB title ID. |
| `_iflix_season` | `int` | Season number (`0` = all seasons). |
| `_iflix_episode` | `int` | Episode number (`0` = all episodes). |
| `_iflix_enabled` | `string` | `'1'` (active in player) or `'0'` (disabled). |
| `_iflix_servers` | `array` | Serialized array of server items: `name`, `url`, `type` (`iframe` or `direct`), `quality`, `language`, `enabled`. |

---

## 👤 3. WordPress User Meta (`wp_usermeta`)

When a viewer registers with WordPress or connects via Firebase:

| Meta Key | Description |
|---|---|
| `_iflix_firebase_uid` | Unique Google Firebase user UID string. |
| `_iflix_subscription_tier` | Active plan ID (`'basic'`, `'standard'`, `'premium'`). |
| `_iflix_subscription_expires` | Unix timestamp of plan expiration date. |

---

## 🔥 4. Google Cloud Firestore Document Tree

```
users/
└── {userId}/
    ├── email: string
    ├── display_name: string
    ├── subscription:
    │   ├── plan_id: "premium"
    │   ├── status: "active"
    │   ├── currency: "$"
    │   ├── started_at: timestamp
    │   └── expires_at: timestamp
    │
    └── profiles/
        └── {profileId}/                 (e.g., "profile_1", "profile_kids")
            ├── name: "Alex"
            ├── avatar: ".../avatar-1.svg"
            ├── isKid: false
            ├── pin: "1234"              (optional parental lock)
            │
            ├── my_list/
            │   └── {tmdb_id}/
            │       ├── id: "550"
            │       ├── media_type: "movie"
            │       ├── title: "Fight Club"
            │       ├── poster_path: "/pB8BM7pdSp6B6Ih7QZ4DrQ3PmJK.jpg"
            │       └── added_at: timestamp
            │
            └── history/
                └── {historyId}/         (e.g., "movie_550" or "tv_1399_s1_e1")
                    ├── id: "550"
                    ├── media_type: "movie"
                    ├── title: "Fight Club"
                    ├── progress_seconds: 2530
                    ├── duration_seconds: 8340
                    └── updated_at: timestamp
```
