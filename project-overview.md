# Project Overview

This document provides a high-level summary of **FarmSpell**, its target audience, core gameplay loop, key features, and project boundaries.

## 1 Game Concept & Vision

### 1.1 Summary
- **1.1.1**: **FarmSpell** is a casual, turn-based farming game designed for children.
- **1.1.2**: It merges farm management (planting, watering, harvesting, selling crops) with educational spelling challenges.
- **1.1.3**: The primary objective is to make spelling practice rewarding and integrated directly into a relaxing casual game loop.

### 1.2 Target Audience & Platform Support
- **1.2.1**: **Target Players**: Children (elementary age) playing independently or with family.
- **1.2.2**: **Supported Hardware**: iPad / mobile touch devices and desktop web browsers.
- **1.2.3**: **Input Methods**: Touch screen controls and native OS keyboard input (soft touch keyboard on mobile/iPad or physical keyboard on desktop).

## 2 High-Level System Pillars

### 2.1 Core Pillars
- **2.1.1**: **Turn-Based Farming Loop**: Action-point-driven farm management governed by an in-game Day Cycle.
- **2.1.2**: **Integrated Spelling Engine**: Action-triggered spelling prompts utilizing active word lists and built-in learning assistance.
- **2.1.3**: **In-Game Economy**: Harvested crop inventory sold at the Shop for coins to purchase seeds and scaling unlocks.
- **2.1.4**: **Profiles & Custom Audio Studio**: Multi-profile device support with in-app microphone recording for custom word pronunciations and browser Text-to-Speech fallbacks.

## 3 Scope Boundaries & Non-Goals

### 3.1 Project Scope Goals
- **3.1.1**: Offline-first execution utilizing Firebase Authentication & Cloud Firestore with offline persistence enabled (`enableIndexedDbPersistence()`).
- **3.1.2**: Device-local audio storage in `IndexedDB` for custom voice recordings with browser Web Speech API (TTS) fallback.
- **3.1.3**: Multi-parent access and device linking via short-lived Share Codes.

### 3.2 Non-Goals
- **3.2.1**: Real-time multiplayer or global competitive leaderboards.
- **3.2.2**: In-app real-money purchases or third-party advertising frameworks.
- **3.2.3**: Cloud storage upload for audio recording Blobs (recordings remain strictly local on device).
- **3.2.4**: Real-time complex farming simulations (e.g., real-world clock timers or weather simulation).
