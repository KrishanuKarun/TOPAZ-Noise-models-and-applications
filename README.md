# TOPAZ v2: Quantum Topology Optimization Framework

**Establishing Topology Optimization as a Paradigm for Noise-Resilient Quantum Circuit Design.**

TOPAZ v2 is a high-performance research framework that treats quantum circuits not as discrete gate sequences, but as **continuous topological architectures**. By navigating the parameter manifold through a $O(2^N)$ Tensor-Dot Engine and a Dual-MMA optimizer, it identifies noise-resilient "Structural Wells" in the landscape. Validated on 156-qubit IBM Heron hardware with a $10.71\times$ fidelity gain, TOPAZ establishes a new frontier for intrinsic resilience in Variational Quantum Algorithms.

[![Main: TOPAZ_V2](https://img.shields.io/badge/Main-TOPAZ-red)](https://doi.org/10.5281/zenodo.20518258)
[![Theory: CTI](https://img.shields.io/badge/Theory-CTI-blue)](https://doi.org/10.5281/zenodo.20516266)
[![Operator: PTE](https://img.shields.io/badge/Operator-PTE-green)](https://doi.org/10.5281/zenodo.20516615)

---

##  Key v2 Breakthroughs

- **The "Structural Well" Discovery**: Empirical proof of hardware-transferable basins in the noise landscape where fidelity is maximized.
- **Dual-MMA Optimizer**: A specialized solver that decouples gradient scales between Intensity ($\rho$) and Timing ($\tau$), preventing scale drowning and ensuring convergence.
- **Tensor-Dot Noise Engine (TDE)**: A high-speed simulation layer delivering a **$3838\times$ wall-clock speedup** ($O(2^N)$ complexity), making deep-circuit optimization tractable.
- **QPU Validation**: Proven on the **156-qubit IBM Heron r2** (`ibm_fez`) hardware.

---

##  Installation

```bash
pip install numpy scipy qiskit["all"] qiskit-aer matplotlib
```

---

##  Framework Architecture & Workflow

TOPAZ v2 is a modular research framework. It follows a hybrid digital-twin optimization path:

1. **Initialization**: Define the target Hamiltonian ($H$) and ansatz topology. Initialize intensity ($\rho$) and timing ($\tau$) parameters.
2. **Reference State**: Establish the noiseless ground truth using the **Tensor-Dot Engine** (Stage 2).
3. **Dual-Track Mitigation**:
   - **Track A (TOPAZ)**: Pre-execution optimization loop finding noise-resilient coordinates via Dual-MMA and Polar Projection ($U_{safe}$).
   - **Track B (ZNE)**: Standard post-circuit observable extrapolation.
4. **Layered Stacking**: Synthesize ZNE Richardson extrapolation on top of the TOPAZ-optimized parameters.
5. **Hardware Validation**: Deploy to live QPU (e.g., `ibm_fez`) for Direct Fidelity Estimation (DFE).

### Procedural Implementation:

```python
# PROCEDURAL LOGIC FOR TOPAZ V2 PIPELINE
PROCEDURE TOPAZ_v2_Research_Pipeline(H_target, problem_type, gate_topology, N_qubits):
    # STAGE 1: Setup
    rho, tau = Initialize_Random_Parameters()
    
    # STAGE 2: Ideal Reference (Tensor-Dot Engine)
    psi_ideal = Compute_Ideal_State(H_target, gate_topology)

    # STAGE 3: Dual-MMA Optimization Loop
    while (iter < 40) and (delta_fidelity > 1e-4) and (stagnation < 5):
        # 3.1 Build U_safe via layer-wise Polar Projection
        U_safe = Apply_Polar_Projection(PTE_Operator(rho, tau))
        
        # 3.2 Evaluation Track (Digital Twin)
        noisy_dm = AerSimulator.run(U_safe, noise_model=FakeFez)
        loss = (1 - Fidelity(noisy_dm, psi_ideal)) + Regularization(rho, tau)
        
        # 3.3 Gradient Update (Subspace Separation)
        # Decouple rho (eps=1e-4) and tau (eps=pi/4)
        grad_rho, grad_tau = Calculate_Gradients(loss)
        rho, tau = Update_MMA(grad_rho, grad_tau)
        
    # STAGE 4: Hardware Validation
    # Final optimal parameters submitted to ibm_fez for DFE validation
```

---

## 📊 Benchmarking Results

| Metric | Baseline (Random) | TOPAZ v2 | Gain / Improvement |
|---|---|---|---|
| **12-q LiH Energy** | $\sim 275.4$ mHa | **$30.78$ mHa** | **$1.87\times$ better than ZNE** |
| **8-q QPU Fidelity** | $0.0155$ | **$0.1660$** | **$10.71\times$ Gain on ibm_fez** |
| **Sim. Speed (N=10)** | $180.4$ s | **$0.047$ s** | **$3838\times$ Speedup** |

---

## 📖 Technical Foundations

This framework is the practical implementation of a three-part theoretical stack:

1. **The Theory**: [CTI Framework](https://doi.org/10.5281/zenodo.20516266) - *A Continuous Topological Interpretation of Quantum Circuits.*
2. **The Operator**: [PTE/UPTE](https://doi.org/10.5281/zenodo.20516615) - *Derivation of Parametric Trotter Expressions.*
3. **The Design**: [TOPAZ v2](https://doi.org/10.5281/zenodo.20518258) - *Establishing Topology Optimization as a Paradigm for Design.*

---

##  Citation

```bibtex
@software{Karun_TOPAZ_v2_2026,
  author = {Karun, Krishanu},
  title = {TOPAZ v2: Establishing Topology Optimization as a Paradigm for Noise-Resilient Quantum Circuit Design},
  year = {2026},
  publisher = {Zenodo},
  doi = {10.5281/zenodo.19214449}
}
```
