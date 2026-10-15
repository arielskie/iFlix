# PHP Hooks & JavaScript Events Reference

**iFlix Stream** provides a comprehensive collection of action hooks and filters allowing WordPress developers to customize core logic, filter media queries, alter video server rankings, and intercept user actions without modifying parent files.

---

## 🪝 WordPress PHP Action Hooks

| Action Hook | Parameters | Description |
|---|---|---|
| `iflix_before_player` | `$post_id, $type, $tmdb_id` | Fires immediately before the video player HTML container renders. |
| `iflix_after_player` | `$post_id, $type, $tmdb_id` | Fires immediately below the video player (ideal for custom related titles or comment widgets). |
| `iflix_before_hero_billboard` | `$hero_items` | Fires above the multi-slide homepage billboard slider. |
| `iflix_after_hero_billboard` | `$hero_items` | Fires below the hero slider before row carousels begin. |
| `iflix_user_registered` | `$user_id, $firebase_uid` | Fires when a user logs in or registers via Firebase credentials. |
| `iflix_subscription_activated` | `$user_id, $plan_data` | Fires when a subscription plan is saved or upgraded. |

### Example Action Hook Implementation
```php
add_action( 'iflix_subscription_activated', function( $user_id, $plan_data ) {
    // Send customized welcome email or trigger Discord/Slack notification
    $user = get_userdata( $user_id );
    $plan_name = $plan_data['plan_name'] ?? 'Premium';
    wp_mail( $user->user_email, 'Welcome to ' . $plan_name . ' on iFlix!', 'Your streaming pass is now active.' );
}, 10, 2 );
```

---

## 🔍 WordPress PHP Filter Hooks

| Filter Hook | Parameters | Default Return | Description |
|---|---|---|---|
| `iflix_tmdb_cache_ttl` | `$ttl_seconds` | `86400` (24h) | Filters the duration in seconds for caching TMDB API responses in transients. |
| `iflix_video_sources` | `$sources, $id, $type` | Array of sources | Modifies the list of available video servers before outputting to the player. |
| `iflix_cinepro_request_args` | `$args, $endpoint` | Array of cURL args | Filters HTTP timeout, headers, or parameters sent to CinePro extractors. |
| `iflix_paywall_enabled` | `$is_enabled, $title_id` | Boolean | Programmatically enable or bypass the paywall for specific promotional titles. |
| `iflix_default_profiles_limit` | `$max_profiles, $tier` | Integer (1 - 6) | Overrides maximum allowed profile count for a specific user tier. |

### Example Filter Hook Implementation
```php
// Make all documentary movies free of paywall restrictions for education
add_filter( 'iflix_paywall_enabled', function( $is_enabled, $title_id ) {
    // Check if title is tagged under documentary
    if ( is_singular( 'movie' ) && has_term( 'documentary', 'genre' ) ) {
        return false; // Free access
    }
    return $is_enabled;
}, 10, 2 );
```

---

## ⚡ Frontend JavaScript Window Events

The client-side `app.js` and `iflix-player.js` emit custom events on the `window` object:

| Event Name | Event Detail Properties | Description |
|---|---|---|
| `iflix:player:ready` | `{ tmdbId, type, season, episode }` | Triggered when the video player finishes initializing and is ready for playback. |
| `iflix:player:progress` | `{ currentTime, duration, percent }` | Triggered every 15 seconds during active playback. |
| `iflix:player:ended` | `{ tmdbId, nextUrl }` | Emitted when video playback reaches the end. |
| `iflix:profile:switched` | `{ profileId, profileName, isKid }` | Emitted when the viewer switches to a different profile. |
| `iflix:mylist:toggled` | `{ id, title, inList }` | Emitted when a title is added to or removed from the user's watchlist. |

### Example Frontend Event Listener
```javascript
window.addEventListener('iflix:mylist:toggled', function(event) {
    console.log('Title updated:', event.detail.title, 'Active status:', event.detail.inList);
});
```
