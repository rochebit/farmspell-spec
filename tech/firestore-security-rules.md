# Technical Specification: Firestore Security Rules

This document specifies the access control policies, permission constraints, and security rules for Cloud Firestore in **FarmSpell**.

## 1 Security Principles

### 1.1 Authentication Requirements
- **1.1.1**: All database read and write operations require an authenticated Firebase user (`request.auth != null`).
- **1.1.2**: Unauthenticated anonymous access to Firestore collections is strictly denied.

### 1.2 Multi-Parent Authorization Policy
- **1.2.1**: A parent user (`request.auth.uid`) can ONLY read, create, or update Player Profile documents where their UID is explicitly listed in `resource.data.authorizedParentUids`.
- **1.2.2**: When creating a new Player Profile, the parent MUST include their own `request.auth.uid` in the initial `authorizedParentUids` array.

## 2 Firestore Security Rules Code

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // Helper function checking if user is an authorized parent
    function isAuthorizedParent(parentUids) {
      return request.auth != null && request.auth.uid in parentUids;
    }

    // Player Profiles Collection
    match /players/{playerId} {
      // Allow read/update/delete if user is listed in authorizedParentUids
      allow read, update, delete: if isAuthorizedParent(resource.data.authorizedParentUids);
      
      // Allow create if authenticated and self UID is included in initial authorizedParentUids
      allow create: if request.auth != null && 
        request.auth.uid in request.resource.data.authorizedParentUids;
    }

    // Share Codes Collection (for linking parents)
    match /shareCodes/{codeId} {
      // Allow read if authenticated
      allow read: if request.auth != null;
      
      // Allow create if creator is authenticated
      allow create: if request.auth != null && 
        request.resource.data.createdByUid == request.auth.uid;

      // Allow update if active and not expired
      allow update: if request.auth != null && 
        resource.data.used == false && 
        request.time < resource.data.expiresAt;
    }
  }
}
```
