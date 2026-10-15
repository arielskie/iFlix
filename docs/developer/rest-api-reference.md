# REST API Reference (`/wp-json/iflix/v1/`)

The **iFlix Stream Core Plugin** exposes a set of REST API endpoints under the namespace `iflix/v1`. These endpoints power client-side dynamic search, modal detail popups, episode list pagination, video streaming source resolution, and notification badges.

---

## 🔑 Authentication & Headers

Most public catalog endpoints are accessible without authentication (`permission_callback => '__return_true'`).
- For authenticated POST requests, pass the WordPress Nonce in the request header:
  ```http
  X-WP-Nonce: <wp_create_nonce('wp_rest')>
  ```
- Base Endpoint: `https://yourdomain.com/wp-json/iflix/v1`

---

## 📡 Endpoints Specification

### 1. Trending Media
Fetches daily or weekly trending titles from TMDB with transient caching.

- **Route**: `GET /wp-json/iflix/v1/trending[/{type}]`
- **Parameters**:
  - `type` (optional, path string): `all`, `movie`, or `tv` (default: `all`).
  - `page` (optional, query int): Page number (default: `1`).
- **Response (200 OK)**:
  ```json
  {
    "page": 1,
    "results": [
      {
        "id": 550,
        "title": "Fight Club",
        "media_type": "movie",
        "poster_path": "/pB8BM7pdSp6B6Ih7QZ4DrQ3PmJK.jpg",
        "backdrop_path": "/hZkgoQYus5vegHoetLkCJzb17zJ.jpg",
        "vote_average": 8.433,
        "release_date": "1999-10-15"
      }
    ],
    "total_pages": 500,
    "total_results": 10000
  }
  ```

---

### 2. Popular Media
Fetches currently popular movies or TV shows.

- **Route**: `GET /wp-json/iflix/v1/popular/{type}`
- **Parameters**:
  - `type` (required, path string): `movie` or `tv`.
  - `page` (optional, query int): Page number (default: `1`).

---

### 3. Top Rated Titles
Fetches critically acclaimed, top-rated movies or series.

- **Route**: `GET /wp-json/iflix/v1/top-rated/{type}`
- **Parameters**:
  - `type` (required, path string): `movie` or `tv`.
  - `page` (optional, query int): Page number.

---

### 4. Title Full Details
Fetches complete details for a movie or TV show, including genres, cast ensemble, and trailer keys.

- **Route**: `GET /wp-json/iflix/v1/details/{type}/{id}`
- **Parameters**:
  - `type` (required, path string): `movie` or `tv`.
  - `id` (required, path int): TMDB item ID.
- **Response (200 OK)**:
  ```json
  {
    "id": 1399,
    "name": "Game of Thrones",
    "overview": "Seven noble families fight for control of the mythical land of Westeros.",
    "number_of_seasons": 8,
    "number_of_episodes": 73,
    "genres": [{"id": 10765, "name": "Sci-Fi & Fantasy"}, {"id": 18, "name": "Drama"}],
    "credits": {
      "cast": [
        {"id": 1223786, "name": "Emilia Clarke", "character": "Daenerys Targaryen"}
      ]
    }
  }
  ```

---

### 5. TV Show Season & Episodes
Fetches the full episode breakdown for a specific season of a TV show. Used by the quick detail modal.

- **Route**: `GET /wp-json/iflix/v1/tv/{id}/season/{season}`
- **Parameters**:
  - `id` (required, path int): TMDB TV ID.
  - `season` (required, path int): Season number (e.g., `1`).
- **Response (200 OK)**:
  ```json
  {
    "season_number": 1,
    "name": "Season 1",
    "episodes": [
      {
        "id": 63056,
        "episode_number": 1,
        "name": "Winter Is Coming",
        "overview": "Jon Arryn, the Hand of the King, is dead...",
        "still_path": "/wrGWeW4WKxnaeA8sxJb2T9O6hm8.jpg",
        "air_date": "2011-04-17"
      }
    ]
  }
  ```

---

### 6. Video Streaming Sources Resolution
Returns the resolved streaming server matrix for a title, factoring in active subscription tier gating and CinePro direct streams.

- **Route**: `GET /wp-json/iflix/v1/sources/{type}/{id}`
- **Parameters**:
  - `type` (required, path string): `movie` or `tv`.
  - `id` (required, path int): TMDB ID.
  - `season` (optional, query int): Season number.
  - `episode` (optional, query int): Episode number.
- **Response (200 OK)**:
  ```json
  {
    "success": true,
    "id": 550,
    "type": "movie",
    "direct_stream": {
      "enabled": true,
      "provider": "cinepro",
      "hls_url": "https://cdn.cinepro.stream/hls/550/master.m3u8",
      "max_resolution": 1080
    },
    "servers": [
      {
        "name": "Server 1 (VidLink HD)",
        "url": "https://vidlink.pro/movie/550",
        "type": "iframe",
        "quality": "1080p"
      },
      {
        "name": "Server 2 (AutoEmbed)",
        "url": "https://autoembed.co/movie/tmdb/550",
        "type": "iframe",
        "quality": "1080p"
      }
    ]
  }
  ```

---

### 7. Instant Search
Executes real-time multi-search queries across TMDB.

- **Route**: `GET /wp-json/iflix/v1/search`
- **Parameters**:
  - `q` (required, query string): Search keywords.
  - `page` (optional, query int): Page number.
