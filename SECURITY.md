# Security Policy

At **iFlix Stream**, security, viewer privacy, and intellectual property protection are top priorities. This document describes the security protocols in place and provides instructions for reporting vulnerabilities.

---

## 🛡️ Supported Versions

We provide active security patches and critical vulnerability fixes for the following releases:

| Version | Supported | Status |
|---|---|---|
| **1.4.x** | ✅ Yes | Current Active Release |
| **1.3.x** | ✅ Yes | Security Patches Only |
| **1.2.x** | ❌ No | Deprecated — Please upgrade to 1.4.x |
| **< 1.2.0** | ❌ No | End of Life (EOL) |

---

## 🔒 Built-in Security Architecture

### 1. Iframe Sandboxing & Anti-Hijack Defense
When running third-party video mirrors or embed sources, iFlix Stream wraps embeds in a restrictive HTML5 sandbox attribute:
```html
sandbox="allow-scripts allow-same-origin allow-forms allow-presentation allow-downloads"
```
This configuration strictly blocks:
- Top-level domain redirection (preventing malicious ad redirects away from your site).
- Unauthorized pop-up windows and malware execution outside the player frame.

### 2. WordPress REST API & Nonce Protection
- Every client-side call to `/wp-json/iflix/v1/` requires a valid WordPress Nonce (`X-WP-Nonce`).
- Admin AJAX endpoints (`wp_ajax_iflix_*`) enforce capability checks (`current_user_can('manage_options')`) alongside localized Nonce verification.

### 3. Firebase Firestore Security Rules
All client-to-Firestore operations are governed by server-side rules preventing unauthorized access to other viewers' profiles, watch histories, or payment statuses:
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
      
      match /profiles/{profileId}/{document=**} {
        allow read, write: if request.auth != null && request.auth.uid == userId;
      }
    }
  }
}
```

### 4. Input Sanitization & Output Escaping
- 100% of external and internal data inputs are processed using WordPress core sanitization APIs (`sanitize_text_field`, `absint`, `esc_url_raw`).
- Database queries utilize prepared statements via `$wpdb->prepare()`.

---

## 🚨 Reporting a Vulnerability

If you discover a security vulnerability or exploit within **iFlix Stream**:

1. **Do NOT open a public GitHub issue** disclosing the vulnerability.
2. Send an email directly to the core development team at `security@ayengcoding.com` or contact the lead architect privately on Telegram at [@ayengcoding](https://t.me/ayengcoding).
3. Please include:
   - Detailed description of the issue and potential impact.
   - Step-by-step reproduction instructions or Proof of Concept (PoC).
   - The affected version(s) of the theme and plugin.
4. You will receive an acknowledgment within **24 hours**, and our team will work to provide a patch within **72 hours** before any public release.
