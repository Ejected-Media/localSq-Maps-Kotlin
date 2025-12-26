Building a DEVELOPMENT.md for your localSq-Maps-Kotlin repository is a strategic move. It acts as the "steering document" that keeps your momentum high, especially when you're alternating between the Python backend and the Kotlin frontend.
Here is a draft for your /Development/DEVELOPMENT.md file, tailored to your "Quantum Paper" and "Hackathon-style" rapid iteration philosophy.

### 🚀 localSq Development Guide 
Rapid Iteration & Experimental Features
This document outlines the workflow for the localSq ecosystem. We prioritize "Spec-First, Then Vibe"—meaning we define the physical requirement (e.g., "Must work in a Ketchum dead zone") before we write the code.

### 🛠 Tech Stack Snapshot 
 * Backend: Python 3.11 + FastAPI (The "Environmental Logic")
 * Frontend: Kotlin + Jetpack Compose (The "Quantum Medium")
 * Sensors: ARCore Geospatial + Maps SDK (The "LiDAR Eyes")
 * Persistence: SQLite via Room (The "Newton Archive")

### 🏗 Development Workflow (The "Crittenden Loop") 
Inspired by the Google SF GameJam, we use a rapid loop to test environmental triggers:
 * Modify Python Biomes: Update main.py in /backend to define new regional rules (e.g., adding a "Volcanic" biome for Oahu).
 * Generate Kotlin Models: Ensure the Ktor data models in the Android app match the new Python schema.
 * Local Field Test: Run the app on a physical device. Use the Developer Dashboard (7-tap secret menu) to toggle the new feature.
 * Stamen Check: Verify if the GPS logs are being rendered artistically in the "Life of the City" view.

### 🧪 Experimental Feature Toggles 
These features are managed in the DeveloperDashboard.kt and should be documented here as they evolve:

| Feature | Toggle ID | Status | Note |
|---|---|---|---|
| LiDAR Pulse | feat_lidar | 🟢 Active | Pulse intensity tied to NASA sensor logic. |
| Stamen Trails | feat_stamen | 🟡 Beta | Testing velocity-based line thickness. |
| Waymo Fleet | feat_waymo | 🔴 WIP | Integrating Jaguar I-PACE icons & smooth routing. |
| Newton Ink | feat_ink | ⚪ Backlog | Legacy handwriting overlay for notes. |

### 📂 Folder Conventions 
 * /Development/backend: The FastAPI source and Python ingest scripts.
 * /Development/app: The main Kotlin source, partitioned by Material layers.
 * /Development/docs: Architecture diagrams and "Quantum Paper" design specs.

### 🏔 Testing in the Wild 
 * Ketchum Mode: Testing offline SQLite persistence.
 * Oahu Mode: Testing Geofencing and Oceanic biome triggers.
 * Phoenix Mode: Testing high-heat thermal performance and 3D building occlusion.

### 🔑 Secret Handshake 
To unlock the Quantum Settings in the app:
 * Navigate to Settings > About.
 * Tap the Version Number 7 times.
 * The Developer Dashboard will slide up from the bottom (Material Modal).

A Next Step for Your Repo
Since you have a Development folder on GitHub, would you like me to show you how to set up a pre-commit hook? This is a small script that runs locally on your computer every time you type git commit, ensuring that your Python models and Kotlin models stay perfectly in sync before the code even leaves your machine.
