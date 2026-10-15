# Developer Hub & Architecture Guide

Welcome to the **Developer Hub** for **iFlix Stream**. This section is intended for software engineers, backend architects, and WordPress developers extending the theme, integrating external services, building custom plugins, or interacting with the iFlix REST API.

---

## 🛠️ Developer Guides in this Section

| Guide | Scope | Description |
|---|---|---|
| [**rest-api-reference.md**](rest-api-reference.md) | REST Endpoints | Complete specification of all `/wp-json/iflix/v1/` routes with parameters and response models. |
| [**hooks-and-filters.md**](hooks-and-filters.md) | PHP & JS Hooks | Reference of core WordPress actions, filters, and client-side lifecycle events. |
| [**database-and-schema.md**](database-and-schema.md) | Storage Schema | Structure of WordPress options, user meta, post meta, and Firestore document paths. |

---

## 🏛️ Core Architecture Principles

1. **Decoupled Architecture**: High-frequency real-time operations (such as progress tracking every 15 seconds) are completely decoupled from MySQL and handled by Google Cloud Firestore.
2. **Micro-Optimized Caching**: Heavy remote HTTP calls to TMDB and video extractors are wrapped in 24-hour transient caches.
3. **Vanilla Modern JavaScript**: Zero bulky external frontend frameworks (React, Angular, Vue, or heavy jQuery) in consumer-facing player pages to ensure blazing fast mobile paint times.
4. **Strict Isolation**: Profile watchlists and Kids PIN locks operate under cryptographic isolation and strict Firestore security rules.
