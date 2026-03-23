# 🌌 DeepLense Simulation Agent (Context-Aware)

A hardened, astrophysicist-AI-powered agent for the **DeepLenseSim** pipeline.

## 🚀 Quick Start

### 1. Prerequisites
- **Ollama**: Install and run Ollama with `llama3.2` pulled:
  ```bash
  ollama run llama3.2
  ```
- **Python 3.9+**

### 2. Installation
```bash
# Set up virtual environment
python -m venv venv
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

## ✨ Unique Features & Scientific Innovations

- **🧠 Contextual Guard (No Hallucination Zone)**: Unlike standard AI agents, this system is **physically prohibited** from guessing parameters. It injects your raw prompt directly into the simulation tool to verify you actually typed the Model and Mass, ensuring 100% human-in-the-loop fidelity.
- **⚛️ Physics-Strict Constraints**: Automated Pydantic-level validation for astrophysical consistency:
    - **Redshift Ordering**: Rejects $z_{source} \leq z_{lens}$ with a scientific explanation.
    - **Redshift Capping**: Hard-capped at $z \leq 1.0$ for instrument consistency.
- **🛠️ Numerical Stabilization Patches**: Injected a global `colossus` background cosmology (`planck15`) in `tools.py`. This prevents the "Interpolation Range" crashes (`x_new < 0.001`) common in custom-redshift simulations.
- **🌈 Model IV Multi-Band Synthesis**: Integrated a dedicated pipeline for **3-channel RGB synthesis** (`g`, `r`, `i` bands) for Model IV Euclid simulations.
- **📂 Bifurcated Data Architecture**: Clear separation between your personal **Verification Gallery (`Tests/`)** and your live **Research Workspace (`output/`)**.

## 🛡️ Hardened Safety Features
- **Hardened Human-in-the-Loop**: The agent is now strictly forbidden from guessing core parameters. It will actively explain constraint violations (e.g., $z_{gal} \leq z_{halo}$) back to the user.
- **Redshift Limits**: Strictly enforces $z_{gal} \leq 1.0$ across all models with Pydantic-level validation.
- **Numerical Stability**: Injected `colossus` global cosmology (`planck15`) to prevent backend interpolation crashes.
- **Model IV Synthesis**: Support for multi-band Euclid images with advanced astro-parameters (`sigma_v`, etc.).

## 📂 Project Structure
- `agent.py`: Main Pydantic AI agent logic & chat loop.
- `tools.py`: Simulation backend with Colossus stability patches.
- `Schemas.py`: Rigid Pydantic validation schemas.
- `DeepLenseSim/`: Submodule containing the core physics library.
- `Tests/`: Static directory of example simulation results (pushed to repo).
- `output/`: Functional directory for subsequent simulation outputs (ignored by git).
- `Tests/README.md`: Master list of 20 verification prompts.


---

*This agent was developed to automate high-fidelity strong lensing research with human-in-the-loop oversight.*
