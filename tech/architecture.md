# Technical Architecture and Stack

This document specifies the technology stack, backend architecture, multi-parent authorization model, storage protocols, and audio boundaries for **FarmSpell**.

## 1 Core Tech Stack Overview

### 1.1 Stack Components
- **1.1.1**: **Frontend Framework**: React + TypeScript + Vite.
- **1.1.2**: **Application Type**: Progressive Web Application (PWA) with Service Worker for 100% offline installation on iPad, mobile devices, and desktop browsers.
- **1.1.3**: **Backend Cloud Services**: Firebase Authentication & Cloud Firestore database.
- **1.1.4**: **Device-Local Storage**: Browser `localStorage` (for device settings) & `IndexedDB` (for local custom audio Blobs).

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
