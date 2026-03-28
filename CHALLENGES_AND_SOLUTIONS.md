# 🛠️ DeepLense Agent: Development Challenges & Solutions

This document details the specific technical hurdles overcome during the development of the DeepLense Simulation Agent.

---

### 1. The `pyHalo` & `Colossus` Stability Crisis
**Problem:** When attempting simulations with custom redshifts (e.g., $z_{halo}=0.3, z_{gal}=0.7$), the backend frequently crashed with an interpolation error:
> `ValueError: A value (0.0) in x_new is below the interpolation range's minimum value (0.001).`

This was caused by the `pyHalo` library's underlying reliance on the `colossus` cosmology library, which was defaulting to a state that didn't support the requested redshift calculation ranges.

**Solution:** We injected a global background cosmology configuration (`planck15`) at the start of the `run_deeplensesim` tool in `tools.py`:
```python
from colossus.cosmology import cosmology
cosmology.setCosmology('planck15')
```
This stabilized the interpolation engine and allowed for robust, wide-range redshift simulations.

---

### 2. Native HST (Model III) Support
**Problem:** The DeepLenseSim core library initially lacked a direct mapping for "Hubble Space Telescope" (HST) in its ObservationConfig loop, which typically defaults to Euclid (Model II).

**Solution:** We manually patched the simulation bridge in `tools.py`:
- We explicitly identified `Model_III` requests.
- We configured a local `ObservationConfig('hst')` within the tool loop to ensure high-resolution HST imaging parameters were applied to the simulation API.

---

### 3. Agent "Hardening" & Hallucination Defense
**Problem:** Using a smaller local model (Llama 3.2), the agent was too "helpful." If a user said "Run a simulation," the agent would hallucinate Model I and 1e12 mass because it wanted to use the tool, and Pydantic had "default" values.

**Solution: Multi-Layer Hardening**
1. **Schema Rigidness**: Removed all default values from `SimulationConfig`. Pydantic now blocks any tool call missing mandatory parameters.
2. **Context-Aware Verification (`AgentDeps`)**: We implemented a "Contextual Guard." The tool now receives the user's raw prompt history and scans it for mandatory keywords (e.g., "Model", "CDM", "Mass"). If it detects the agent is guessing a parameter that wasn't in the chat, it **rejects the tool call** and returns a request for clarification.

---

### 4. Model IV 3-Channel Synthesis
**Problem:** Synthesizing multi-color Euclid empirical results required processing different bands simultaneously.

**Solution:** We built a dedicated synthesis loop for Model IV that:
1. Iterates through three bands (Green, Red, Infra-red).
2. Sets unique observation offsets (`source_pos_x`, `source_pos_y`).
3. Merges the results into a high-fidelity synthetic image stored in the `Tests/` gallery.

---

### 5. Repository Bifurcation
**Problem:** Balancing the need for "Proof of Work" (pushed results) vs "New Research" (clean local structure).

**Solution:** We refactored the project structure into:
- **`Tests/`**: A permanent, version-controlled library of verification results.
- **`output/`**: A clean, git-ignored workspace for all future research runs.
