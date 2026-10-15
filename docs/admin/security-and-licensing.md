# Security, Licensing & Content Protection

This guide outlines the security hardening features, content protection mechanisms, and licensing architecture integrated into **iFlix Stream**.

---

## 🛡️ Content Protection & Anti-Theft

### 1. Iframe Sandbox Policy
When serving third-party video mirrors, iFlix Stream wraps embeds in an HTML5 sandbox container:
```html
<iframe src="..." 
        sandbox="allow-scripts allow-same-origin allow-forms allow-presentation allow-downloads" 
        allow="autoplay; encrypted-media; fullscreen" 
        frameborder="0">
</iframe>
```
**Benefits**:
- **Prevents Top-Level Navigation**: Disallows malicious scripts from redirecting the user's browser tab away from your domain.
- **Blocks Unauthorized Popups**: Eliminates annoying multi-tab pop-unders common to third-party ad networks.

### 2. Client-Side DevTools Defense (Optional)
Configurable in **iFlix Stream** -> **Settings** -> **Security**:
- **Disable Right-Click Context Menu**: Prevents casual visitors from inspecting video elements or saving media assets directly.
- **Debugger Trapping**: Optional script loop that halts execution if browser developer tools (F12) are detected during active video streaming.

---

## 🔑 Licensing & Domain Locking

The theme includes a lightweight domain licensing validation system managed via `IFLIX\Core\Admin\License`:
- **Domain Binding**: Validates that the active host matches the domains registered under your commercial purchase.
- **Localhost & Staging Exemption**: Automatically permits `localhost`, `127.0.0.1`, and common `.local` / `.test` domains without consuming a production activation slot.
- **License Status**: Displays license key, active status, expiration date, and customer email directly in the admin header.
