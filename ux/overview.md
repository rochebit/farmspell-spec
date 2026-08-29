# User Experience (UX) Overview

This document provides a high-level overview of the user interfaces required for **FarmSpell** and specifies the functional components each interface must include.

## 1 UX Principles & Design Goals

### 1.1 Core Principles
- **1.1.1**: **Kid-Friendly Simplicity**: Interfaces must be clear, uncluttered, and immediately understandable for elementary-age children.
- **1.1.2**: **Immediate Visual Feedback**: Farm plot states, energy consumption, and spelling attempts must provide instant visual and audio feedback.
- **1.1.3**: **Cross-Platform Input**: Seamlessly support touch interactions on mobile/iPad and physical keyboard/mouse input on desktop browsers.
- **1.1.4**: **Modular Component Architecture**: For view-level composition mapping, see [ux/view-and-component-contents.md](view-and-component-contents.md). For detailed input schemas, states, and event triggers of individual UI components, see [ux/components/](components/README.md).

## 2 Required Interface Views & Functional Components

### 2.1 Main Farm View
- **2.1.1 Purpose**: The primary interactive game screen where players manage their farm grid and initiate actions.
- **2.1.2 Required Functional Components**:
  - **Player & Status Bar**: Displays current active profile name/avatar, current coin balance, remaining daily action points, and navigation links.
  - **Farm Grid Area**: Renders all 25 farm plots with distinct visual indicators for plot states (`Empty`, `Planted`, `Growing`, `ReadyToHarvest`, `Withered`).
  - **Farmhouse / Day Control**: Interactive structure triggering the "End Day / Go to Sleep" day transition.
  - **Seed Selection Interface**: Interface allowing the player to select which seed type to sow when planting on an empty plot.

### 2.2 Spelling Challenge Interface
- **2.2.1 Purpose**: Modal overlay presented whenever a farm action requires spelling a word.
- **2.2.2 Required Functional Components**:
  - **Audio Playback Control**: Prominently accessible button to re-play the target word audio pronunciation.
  - **Target Word Slot Display**: Visual container rendering empty slots corresponding to target word length, filling with typed uppercase letters as the child types.
  - **Native Keyboard Focus Region**: Hidden or explicit focus area that invokes the native OS soft keyboard on mobile/iPad or listens for desktop keypresses.
  - **Learning Assistance Overlay**: Temporary display window presented after 2 failed attempts that shows the correct target word and re-plays audio while input is temporarily locked.
  - **Cancellation Control**: Exit/Cancel button allowing the child to close the challenge without completing the farm action (consuming 1 Action Point).

### 2.3 Shop Interface
- **2.3.1 Purpose**: Interface for selling harvested crops and buying new seed packets.
- **2.3.2 Required Functional Components**:
  - **Crop Selling Controls**: Instant-sell controls listing harvested crops, unit values, and sell buttons (0 action cost, 0 spelling prompts).
  - **Seed Purchase Controls**: Storefront listing available seed types, coin costs, and growth requirements.
  - **Coin Balance Indicator**: Real-time display of current player coins.

### 2.4 Audio Studio Interface
- **2.4.1 Purpose**: Interface for children or parents to record custom voice pronunciations for word lists.
- **2.4.2 Required Functional Components**:
  - **Word List Navigator**: Browser for selecting word packs and viewing individual words.
  - **Microphone Recorder Controls**: Interactive controls for Record, Stop, Test Playback, and Save.
  - **Recording Duration Timer**: Visual timer indicator showing progress during the 5.0-second maximum recording window.
  - **Recording Status Indicators**: Clear badges indicating whether a word has a custom voice recording or uses default TTS.

### 2.5 Profile & Parent Management Interface
- **2.5.1 Purpose**: Interface for switching player profiles and linking multiple parent accounts across devices.
- **2.5.2 Required Functional Components**:
  - **Profile Switcher**: Screen for selecting between active child profiles on the device or creating a new profile.
  - **Share Code Generator**: Control for generating a short-lived 6-digit code to share a child profile with another parent.
  - **Share Code Redeemer**: Input control for entering a 6-digit share code to link an existing child profile to the current parent account.
