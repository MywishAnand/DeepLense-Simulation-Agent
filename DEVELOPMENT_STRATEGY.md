# 🧩 DeepLense Agent: Development Strategy & Roadmap

This document outlines the high-level strategy and architectural evolution used to build the DeepLense Simulation Agent.

---

### Phase 1: Foundation (Establishing the Scientific Baseline)
**Goal:** Create a functional bridge between Large Language Models and the DeepLenseSim physics library.
- **Strategy:** Leverage **Pydantic AI** to create a tool-centric agent. By using Pydantic schemas, we enforced type-safety for complex astrophysical parameters (mass, redshift, substructure).
- **Outcome:** A basic "Model I" and "Model II" executor that could take natural language and run a single simulation.

---

### Phase 2: Feature Expansion (Maximum Telescope Coverage)
**Goal:** Support the full range of DeepLenseSim models (I–IV) and astronomical instruments.
- **Strategy:** 
    - **Model III (HST)**: Patched the simulation backend to support Hubble Space Telescope instrument configurations natively.
    - **Model IV (Multi-band)**: Engineered a synthetic synthesis loop that generates three distinct observation bands (g, r, i) and merges them into a scientific composite.
- **Outcome:** A versatile agent capable of simulating Euclid, HST, and multi-band empirical data.

---

### Phase 3: Physics Hardening (Numerical Stability)
**Goal:** Ensure the agent survives custom research configurations.
- **Strategy:** Deep-debugged backend crashes occurring at non-standard redshifts. Identified a failure in the `colossus` cosmology library's default interpolation.
- **Execution:** Injected a global background cosmology (`planck15`) at the tool level to ensure numerical stability regardless of the user's chosen $z_{halo}$ and $z_{gal}$.
- **Outcome:** A robust simulation backend that no longer crashes on custom redshift inputs.

---

### Phase 4: Agent Security (The "Hallucination Defense")
**Goal:** Transition from a "helpful instrument" to a "disciplined researcher."
- **Strategy:** Addressed the common failure mode of small LLMs (like Llama 3.2) where they "guess" parameters missing from a user's prompt.
- **Innovation (Context-Aware Guard)**: 
    1. **Zero-Default Schema**: Removed all default values to trigger Pydantic validation failures on missing data.
    2. **Dependency Injection (`AgentDeps`)**: Injected the raw user prompt into the tool logic. The tool now physically scans for mandatory keywords (Model, Substructure, Mass). 
- **Outcome:** An agent that **refuses to guess** and instead asks for clarification, ensuring 100% human-in-the-loop fidelity.

---

### Phase 5: Professional Repository Architecture
**Goal:** Provide a clean, reproducible, and verifiable handoff.
- **Strategy:** 
    - **Data Bifurcation**: Separated static verification assets (stored in `Tests/`) from dynamic new research outputs (stored in `output/`).
    - **Documentation Stack**: Created a hierarchical documentation set (README, Challenges, Strategy) to make the project "GitHub-Ready."
- **Outcome:** A production-grade repository with a one-click startup (`start_agent.sh`).
