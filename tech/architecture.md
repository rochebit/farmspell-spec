# Technical Architecture and Stack

This document specifies the technology stack, backend architecture, multi-parent authorization model, storage protocols, and audio boundaries for **FarmSpell**.

## 1 Core Tech Stack Overview

### 1.1 Stack Components
- **1.1.1**: **Frontend Framework**: React + TypeScript + Vite.
- **1.1.2**: **Application Type**: Progressive Web Application (PWA) with Service Worker for 100% offline installation on iPad, mobile devices, and desktop browsers.
- **1.1.3**: **Backend Cloud Services**: Firebase Authentication & Cloud Firestore database.
- **1.1.4**: **Device-Local Storage**: Browser `localStorage` (for device settings) & `IndexedDB` (for local custom audio Blobs).

### 1.2 Automated Build Versioning & Commit Tracking
- **1.2.1 Version Generation**: Every build and development instance dynamically derives an incremental version string tied to the latest Git commit:
  - Format: `v0.1.<commitCount> (<shortHash>)` (e.g., `v0.1.48 (b4e79a6)`).
  - Automatically increments with every new commit.
- **1.2.2 Build-Time Injection**: Vite injects the version string at build/runtime via `define: { __APP_VERSION__: JSON.stringify(version) }` or `import.meta.env.VITE_APP_VERSION`.
- **1.2.3 Persistent Screen Display**: The application renders the version string in the bottom corner of the main viewport to provide immediate visibility of whether the client is running the latest deployment.
- **1.2.4 Manual Force Update & Cache-Purge Routine**: Tapping the `VersionBadge` executes an immediate client cache bust:
  - **Cache Purge**: Wipes all entries in browser `CacheStorage` (`caches.delete()`).
  - **Service Worker Reset**: Unregisters existing Service Worker registrations (`registration.unregister()`) or posts `SKIP_WAITING`.
  - **Hard Navigation Reload**: Forces a cache-bypassing navigation reload (`window.location.href = origin + pathname + '?t=' + Date.now()`).
  - **Data Safety**: Preserves all local player profile and audio storage (`IndexedDB` / Firestore offline database) untouched during cache purge.

### 1.3 Mobile Debug Console & Diagnostics
- **1.3.1 Activation via URL Parameter**: Debug mode activates whenever the URL contains query parameter `?debug=true` or `?dev=true` (e.g., `https://farmspell.app/?debug=true`).
- **1.3.2 Session Persistence**: When activated via URL, debug mode state is persisted in `sessionStorage` (`farmspell_debug_mode = "true"`) so subsequent internal navigations and reloads retain debug logging until the browser session ends.
- **1.3.3 Console Interception & Error Trapping**:
  - The client intercepts all calls to `console.log`, `console.info`, `console.warn`, `console.error`, as well as global `window.onerror` and `window.onunhandledrejection` events.
  - Native console logging behavior is preserved.
  - Captured log objects store `timestamp: number`, `level: 'log' | 'info' | 'warn' | 'error'`, `message: string`, and `stack?: string`.
- **1.3.4 Memory Cap (Ring Buffer)**: Captured logs are held in a fixed-size ring buffer capped at **300 entries** to prevent memory leaks during long mobile play sessions.
- **1.3.5 On-Screen Diagnostics Overlay**: When active, renders a collapsible on-screen debug panel (`DebugLogOverlay`) with log filtering, live log streaming, and single-tap copy-to-clipboard for iPad/mobile troubleshooting.

## 2 Firebase Backend & Multi-Parent Authorization

### 2.1 Firebase Authentication
- **2.1.1**: Parents/Guardians sign in using Firebase Authentication exclusively via **Google Sign-In** (`GoogleAuthProvider`). Email/password and other third-party auth providers are not supported.
- **2.1.2**: Each authenticated parent receives a unique Firebase `auth.uid`.

### 2.2 Shared Player Model (`authorizedParentUids`)
- **2.2.1**: Player profiles exist in a top-level Firestore collection at `/players/{playerId}`.
- **2.2.2**: Each Player document contains an array field `authorizedParentUids: string[]` containing the UIDs of all parents/guardians authorized to view and manage that child (see [data-structures.md](data-structures.md)).

### 2.3 Firestore Queries & Security Rules
- **2.3.1**: When a parent signs in, the client queries for authorized players using the `array-contains` operator:
  ```javascript
  const q = query(
    collection(db, "players"),
    where("authorizedParentUids", "array-contains", currentUser.uid)
  );
  ```
- **2.3.2**: Access control is enforced by Firestore Security Rules (see [firestore-security-rules.md](firestore-security-rules.md)).

### 2.4 Parent Linking via Join Code Handshake
- **2.4.1**: When a new parent (Parent B) signs into their device, they tap "Connect to a Child" to generate a short-lived 6-digit Join Code (e.g., `JOIN-8492`, valid for 15 minutes) saved in `/joinCodes/{code}` containing Parent B's UID and display name.
- **2.4.2**: Parent B shares the 6-digit code with an existing authorized parent (Parent A).
- **2.4.3**: Parent A enters the code in their Child Settings. Parent A's client fetches `/joinCodes/{code}` by its exact ID, extracts Parent B's UID, and directly updates `/players/{playerId}` by appending Parent B's UID to `authorizedParentUids`.
- **2.4.4**: Because Parent A is already authorized on `/players/{playerId}`, this write executes client-side without requiring Cloud Functions or security rule bypasses. Parent B's real-time query instantly discovers the linked child profile.

## 3 Offline Execution & Audio Storage Boundaries

### 3.1 Offline Firestore Persistence
- **3.1.1**: The web application initializes Firestore offline persistence (`enableIndexedDbPersistence()`).
- **3.1.2**: All farm actions, day progression, and inventory updates save to the offline cache instantly when offline, and automatically sync to Cloud Firestore when network connectivity is restored.

### 3.2 Device-Local Audio Storage (IndexedDB)
- **3.2.1**: **Local Audio Rule**: Custom voice recordings made in the Audio Studio are stored EXCLUSIVELY in the device's local `IndexedDB` database (`FarmSpellAudioDB`).
- **3.2.2**: Audio Blobs are NOT uploaded to Firebase/Cloud Storage to ensure zero cloud audio costs, instant local playback, and device privacy.
- **3.2.3**: Primary Key Format in IndexedDB: `${playerId}_${wordId}`.

### 3.3 Text-to-Speech Fallback
- **3.3.1**: If a player opens a spelling challenge on a device that lacks a local custom audio recording for the word, the app automatically invokes the browser's Web Speech API (`window.speechSynthesis`).
