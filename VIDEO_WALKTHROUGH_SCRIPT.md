# 🎥 DeepLense AI Agent: Comprehensive Video Walkthrough & Technical Study

This document provides a detailed, technical explanation of every core component in this project. Use this as a script and reference for your video walkthrough.

---

## 🏗️ 1. The "Hardened" Agent: `agent.py`
This is the central "brain" of the project. It uses the **Pydantic AI** framework to bridge a local **Ollama (Llama 3.2)** model with our scientific simulation tools.

### **Key Functionalities:**
- **Contextual Guard (`AgentDeps`)**: This is our most unique innovation. Smaller local models often "guess" parameters like `model_type` or `main_halo_mass` just to make a tool call. To stop this, we pass your **raw prompt text** directly into the tool. The tool then scans your actual words for mandatory keywords. If you didn't say it, the agent can't use it!
- **System Prompt Integrity**: The prompt defines the agent's identity as a disciplined astrophysicist. It explicitly forbids guessing and forces clarification questions whenever a parameter is absent or physically impossible.
- **Retries & Error Catching**: The agent is configured to catch Pydantic validation errors (like redshift ordering) and translate them into a helpful conversational response for the user.

---

### 🦴 2. The Physical Skeleton: `schemas.py`
This file defines the **SimulationConfig**—the strict mathematical rules for what constitutes a valid simulation request.

### **Key Functionalities:**
- **Zero-Default Hardening**: Every mandatory field (`model_type`, `substructure`, `mass`, `z_halo`, `z_gal`) has **no default value**. If any of these are missing, the tool call logically cannot proceed.
- **Redshift Validation (`validate_redshifts`)**: We implemented a `@model_validator` that enforces the astrophysical rule: $Z_{source}$ must be strictly greater than $Z_{lens}$. It also caps $Z_{source}$ at $1.0$ to remain within the high-fidelity range of the DeepLenseSim instrument configs.
- **Type Safety**: Enums are used for `ModelType` and `SubstructureType`, preventing the AI from inventing non-existent models.

---

### 💪 3. The Execution Engine: `tools.py`
This file contains the "muscles"—the logic that interfaces with `DeepLenseSim`, `lenstronomy`, and `pyHalo`.

### **Key Functionalities:**
- **Stability Patch (Colossus)**: Early in development, we found that custom redshifts caused the backend to crash. We fixed this by injecting a global `planck15` cosmology configuration. This provides a stable background for all distance calculations.
- **Model III (HST) Mapping**: Since `DeepLenseSim` didn't natively map "HST" in the local loop, we patched the tool to manually configure an `ObservationConfig('hst')` whenever a Model III request is detected.
- **Model IV RGB Synthesis**: This is a custom pipeline we built. It runs three separate simulations (Green, Red, Infra-red bands), applies unique offsets, and merges them into a premium synthetic image.
- **Bifurcated Directory Logic**: It separates static verification results (which go to `Tests/`) from new, dynamic user work (which go to `output/`).

---

### 🖼️ 4. The Verification Gallery: `Tests/`
This is not just a folder; it is a **Live Scientific Documentation**.

### **Key Functionalities:**
- **`README.md` (moved from `tests.md`)**: Contains the 20 master prompts used to "stress-test" the agent.
- **Results Persistence**: All 64 constituent results (npy/png) are committed to the repository. This allows anyone on GitHub to see the "ground truth" of what the agent is capable of.

---

### 🎨 5. The Cosmic Interface: `index.html`, `style.css`, `script.js`
The front-end is designed to be a **premium showcase** for your GitHub Pages.

### **Key Functionalities:**
- **Glassmorphism**: A modern UI design language using translucent panels, blurs, and subtle borders.
- **Cosmic Animations**: CSS `@keyframes` create the "falling stars" effect, and the `Intersection Observer` in `script.js` creates a "reveal" effect as you scroll down the page.
- **Mock Interaction**: The dashboard includes a hardcoded chat demo that teaches users how the "Hardened" Agent thinks and asks for clarification.

---

### ⚙️ 6. The Automation Nervous System: `start_agent.sh`
This script provides the final "Deploy" experience.

### **Key Functionalities:**
- **Auto-Environment**: It checks for a Python virtual environment and creates one if missing.
- **Auto-Dependency**: It installs `requirements.txt` with the exact pinned versions (`pydantic==2.12.5`, `numpy==1.26.4`, etc.) to ensure reproducibility.
- **Ollama Check**: It performs a health check on `localhost:11434` to make sure your local AI is ready before launching the agent.

---
**Walkthrough Summary**: This project is a masterclass in combining **strict physical constraints** with **modern agentic workflows** and **premium UI design**.
