# Technical Specification: Data Structures & Schemas

This document defines the core data structures, Firestore document schemas, and IndexedDB local object schemas for **FarmSpell**.

## 1 Cloud Firestore Schemas

### 1.1 Player Profile Schema (`/players/{playerId}`)
- **1.1.1 Document Path**: `/players/{playerId}`
- **1.1.2 Schema Definition**:

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "PlayerProfileDocument",
  "type": "object",
  "required": [
    "playerId",
    "playerName",
    "avatarIcon",
    "authorizedParentUids",
    "createdAt",
    "farmState",
    "inventory",
    "unlockedCropIds",
    "activeWordPackIds"
  ],
  "properties": {
    "playerId": {
      "type": "string",
      "description": "Unique UUID v4 identifier for the child player profile"
    },
    "playerName": {
      "type": "string",
      "minLength": 1,
      "maxLength": 30
    },
    "avatarIcon": {
      "type": "string",
      "description": "Identifier for the player avatar icon"
    },
    "authorizedParentUids": {
      "type": "array",
      "items": { "type": "string" },
      "description": "Array of Firebase Auth UIDs for authorized parents/guardians"
    },
    "createdAt": {
      "type": "integer",
      "minimum": 0,
      "description": "Epoch timestamp in milliseconds"
    },
    "farmState": {
      "type": "object",
      "required": ["currentDay", "dailyActionsRemaining", "maxDailyActions", "plots"],
      "properties": {
        "currentDay": { "type": "integer", "minimum": 1 },
        "dailyActionsRemaining": { "type": "integer", "minimum": 0 },
        "maxDailyActions": { "type": "integer", "minimum": 1, "default": 10 },
        "plots": {
          "type": "array",
          "items": {
            "type": "object",
            "required": [
              "plotId",
              "gridIndex",
              "state",
              "cropId",
              "wateredDaysCount",
              "consecutiveUnwateredDays",
              "wateredToday"
            ],
            "properties": {
              "plotId": { "type": "string" },
              "gridIndex": { "type": "integer", "minimum": 0, "maximum": 24 },
              "state": {
                "type": "string",
                "enum": ["Empty", "Planted", "Growing", "ReadyToHarvest", "Withered"]
              },
              "cropId": { "type": ["string", "null"] },
              "wateredDaysCount": { "type": "integer", "minimum": 0 },
              "consecutiveUnwateredDays": { "type": "integer", "minimum": 0 },
              "wateredToday": { "type": "boolean" }
            }
          }
        }
      }
    },
    "inventory": {
      "type": "object",
      "required": ["coins", "seeds", "crops"],
      "properties": {
        "coins": { "type": "integer", "minimum": 0 },
        "seeds": {
          "type": "object",
          "additionalProperties": { "type": "integer", "minimum": 0 }
        },
        "crops": {
          "type": "object",
          "additionalProperties": { "type": "integer", "minimum": 0 }
        }
      }
    },
    "unlockedCropIds": {
      "type": "array",
      "items": { "type": "string" },
      "default": ["crop_wheat"],
      "description": "Array of cropId strings permanently unlocked for purchase in the Shop"
    },
    "activeWordPackIds": {
      "type": "array",
      "items": { "type": "string" },
      "description": "Identifiers of active built-in curriculum packs"
    },
    "customWordLists": {
      "type": "array",
      "items": {
        "type": "object",
        "required": ["listId", "name", "words", "isActive", "createdAt", "updatedAt"],
        "properties": {
          "listId": { "type": "string" },
          "name": { "type": "string", "maxLength": 50 },
          "words": {
            "type": "array",
            "items": {
              "type": "string",
              "pattern": "^[A-Z]{3,10}$"
            },
            "minItems": 1
          },
          "isActive": { "type": "boolean" },
          "createdAt": { "type": "integer" },
          "updatedAt": { "type": "integer" }
        }
      },
      "description": "Parent-created custom spelling word lists"
    },
    "spellingStats": {
      "type": "object",
      "additionalProperties": {
        "type": "object",
        "required": ["correctCount", "incorrectCount", "streak", "lastPracticedAt"],
        "properties": {
          "correctCount": { "type": "integer", "minimum": 0 },
          "incorrectCount": { "type": "integer", "minimum": 0 },
          "streak": { "type": "integer", "minimum": 0 },
          "lastPracticedAt": { "type": "integer" }
        }
      },
      "description": "Spelling performance and streak tracking keyed by uppercase word (e.g. 'CARROT')"
    }
  }
}
```

### 1.2 Join Code Schema (`/joinCodes/{code}`)
- **1.2.1 Document Path**: `/joinCodes/{code}` (where `code` is a 6-digit alphanumeric string, e.g., `"JOIN-8492"`)
- **1.2.2 Schema Definition**:

```json
{
  "code": "JOIN-8492",
  "parentUid": "uid_parent_b",
  "parentDisplayName": "Sarah",
  "createdAt": 1770634800000,
  "expiresAt": 1770635700000
}
```

## 2 IndexedDB Local Audio Schema

### 2.1 Local Object Store Definition
- **2.1.1 Database Name**: `FarmSpellAudioDB`
- **2.1.2 Object Store Name**: `audio_records`
- **2.1.3 Composite Key Format**: `${playerId}_${wordId}`
- **2.1.4 Schema Definition**:

```typescript
interface LocalAudioRecord {
  recordKey: string;        // Composite Key: `${playerId}_${wordId}`
  playerId: string;         // UUID matching PlayerProfile.playerId
  wordId: string;           // Target Word identifier
  audioBlob: Blob;          // Raw audio recording binary Blob
  mimeType: string;         // E.g., 'audio/webm' or 'audio/mp4'
  byteSize: number;         // File size in bytes
  durationSeconds: number;  // Hard capped at 5.0 seconds
  updatedAt: number;        // Epoch timestamp in milliseconds
}
```
