# Firebase Authentication & Firestore Setup

**iFlix Stream** connects directly to **Google Firebase** to provide instant viewer authentication and real-time cloud database synchronization. By offloading user bookmarks, watch history, liked titles, and custom profiles to **Cloud Firestore**, your WordPress server remains fast, lightweight, and capable of handling thousands of concurrent viewers with zero database bottlenecks.

---

## 🔥 1. Creating a Firebase Project

1. Navigate to the [Firebase Console](https://console.firebase.google.com/) and log in with your Google account.
2. Click **Add project** (or **Create a project**).
3. Enter a project name (e.g., `iflix-stream-vod`).
4. Disable or enable Google Analytics (optional, based on preference), then click **Create project**.
5. Once your project is ready, click **Continue**.

---

## 👥 2. Enabling Authentication Providers

1. In the left-hand navigation menu of the Firebase Console, go to **Build** -> **Authentication**.
2. Click **Get Started**.
3. Under the **Sign-in method** tab, enable the desired identity providers:
   - **Email/Password**: Click Email/Password, toggle **Enable**, and click **Save**.
   - **Google**: Click Google, toggle **Enable**, select your project support email, and click **Save**.
4. Under **Settings** -> **Authorized domains**, ensure your WordPress domain is listed (e.g., `streaming.yourdomain.com` and `localhost`).

---

## 🗄️ 3. Initializing Cloud Firestore

1. In the Firebase Console, navigate to **Build** -> **Firestore Database**.
2. Click **Create database**.
3. Choose a Firestore location close to your primary audience (e.g., `us-central1`, `europe-west1`, or `asia-east1`).
4. Select **Start in production mode** and click **Create**.

---

## 🛡️ 4. Deploying Firestore Security Rules

Navigate to the **Rules** tab in Cloud Firestore, replace the default contents with the production security rules below, and click **Publish**:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    
    // User root document and profile collections
    match /users/{userId} {
      // Only the authenticated account owner can read/write their user record
      allow read, write: if request.auth != null && request.auth.uid == userId;
      
      // Profiles subcollection (Profile 1, Profile 2, Kids, etc.)
      match /profiles/{profileId} {
        allow read, write: if request.auth != null && request.auth.uid == userId;
        
        // My List (Watchlist) per profile
        match /my_list/{itemId} {
          allow read, write: if request.auth != null && request.auth.uid == userId;
        }
        
        // Watch History & Resume Progress per profile
        match /history/{historyId} {
          allow read, write: if request.auth != null && request.auth.uid == userId;
        }
        
        // Liked Titles per profile
        match /likes/{likeId} {
          allow read, write: if request.auth != null && request.auth.uid == userId;
        }
      }
    }
    
    // Public read-only notifications broadcast collection
    match /system_notifications/{notifId} {
      allow read: if true;
      allow write: if false; // Only admin service accounts or backend can write
    }
  }
}
```

---

## 📲 5. Registering the Web App in Firebase

1. In your Firebase Console, click the **Gear Icon (⚙️)** next to Project Overview and select **Project settings**.
2. Under **General**, scroll down to the **Your apps** card and click the **Web icon (`</>`)**.
3. Enter an app nickname (e.g., `iFlix WebApp`) and click **Register app**.
4. Firebase will display your `firebaseConfig` object:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyBxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
  authDomain: "iflix-stream-vod.firebaseapp.com",
  projectId: "iflix-stream-vod",
  storageBucket: "iflix-stream-vod.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abcdef1234567890"
};
```

---

## ⚙️ 6. Connecting Firebase Credentials in WordPress

1. In your WordPress admin dashboard, navigate to **iFlix Stream** -> **Settings**.
2. Click on the **Firebase** tab.
3. Copy the individual keys from your `firebaseConfig` into the corresponding settings fields:
   - **Firebase API Key**: `apiKey`
   - **Auth Domain**: `authDomain`
   - **Project ID**: `projectId`
   - **Storage Bucket**: `storageBucket`
   - **Messaging Sender ID**: `messagingSenderId`
   - **App ID**: `appId`
4. Click **Save Changes**.

---

## 🔄 7. Firestore Document Hierarchy Architecture

```mermaid
erDiagram
    USERS ||--o{ PROFILES : contains
    PROFILES ||--o{ MY_LIST : saves
    PROFILES ||--o{ HISTORY : tracks
    PROFILES ||--o{ LIKES : rates

    USERS {
        string uid PK
        string email
        string display_name
        timestamp created_at
        object subscription "plan_id, status, expires_at"
    }

    PROFILES {
        string profile_id PK
        string name
        string avatar
        boolean isKid
        string pin "4-digit parental hash"
    }

    MY_LIST {
        string item_id PK
        int tmdb_id
        string media_type "movie or tv"
        string title
        string poster_path
        timestamp added_at
    }

    HISTORY {
        string episode_id PK
        int tmdb_id
        int season
        int episode
        float progress_seconds
        float duration_seconds
        timestamp updated_at
    }

    LIKES {
        string title_id PK
        string title
        timestamp liked_at
    }
```
