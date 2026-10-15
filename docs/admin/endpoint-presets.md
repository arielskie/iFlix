# Endpoint Presets Manager

The **Endpoint Presets Manager** allows administrators to define custom, highly granular TMDB API queries without writing any backend code. Once saved, these presets become immediately selectable as data sources inside the **Section Builder** or can be queried directly via the iFlix REST API.

---

## 🧭 Location in WordPress

Navigate to **iFlix Stream** -> **Endpoint Presets** in `wp-admin`.

---

## 🛠️ Anatomy of an Endpoint Preset

Each preset consists of a human-readable name, media type, and TMDB query parameters:

| Configuration | Field Name | Description / Example |
|---|---|---|
| **Preset Identifier** | `preset_key` | Unique machine-readable slug (e.g. `korean_romance`, `top_anime`). |
| **Display Name** | `name` | Human-readable label (e.g. `Heartfelt Korean Romances`). |
| **Content Type** | `media_type` | `movie` or `tv`. |
| **API Path** | `endpoint` | Base TMDB route (usually `/discover/movie` or `/discover/tv`). |
| **Query Parameters** | `query_params` | URL query parameters passed directly to TMDB Discover. |

---

## 🎯 High-Performance Curated Preset Recipes

Here are production-tested query parameter presets you can copy and paste directly into your admin:

### 1. Trending Anime Series
- **Content Type**: `TV Series`
- **Endpoint**: `/discover/tv`
- **Query Parameters**:
  ```http
  with_genres=16&with_original_language=ja&sort_by=popularity.desc&vote_count.gte=50
  ```

### 2. High-Octane Action Blockbusters
- **Content Type**: `Movie`
- **Endpoint**: `/discover/movie`
- **Query Parameters**:
  ```http
  with_genres=28,12&sort_by=revenue.desc&vote_average.gte=6.5&vote_count.gte=500
  ```

### 3. Top-Rated Korean Dramas
- **Content Type**: `TV Series`
- **Endpoint**: `/discover/tv`
- **Query Parameters**:
  ```http
  with_original_language=ko&with_genres=18&sort_by=vote_average.desc&vote_count.gte=100
  ```

### 4. Psychological Thrillers & Mystery
- **Content Type**: `Movie`
- **Endpoint**: `/discover/movie`
- **Query Parameters**:
  ```http
  with_genres=53,9648&sort_by=popularity.desc&vote_average.gte=7.0
  ```

---

## 🧪 Real-Time In-Admin Testing

Before assigning a preset to your live homepage:
1. Enter your query parameters in the preset row.
2. Click **Test Endpoint**.
3. iFlix Stream will execute an immediate remote query against TMDB and display a live JSON preview modal showing the count of matching titles and sample posters.
