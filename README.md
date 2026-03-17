# TOPAZ: Custom Noise Models & Optimizer

## Install

```bash
pip install numpy scipy qiskit["all"] matplotlib
```

- Use `qiskit["all"]` for most libraries at once, and for future projects with Qiskit like VQE and QAOA.

## Run

```bash
python TOPAZ + noise.ipynb
```

## What It Does

- **Custom Noise**: Sets up three channels of correlated noise by manually creating Kraus operators and setting spatial intensity decay variables.
- **TOPAZ Optimizer**: 8-qubit quantum circuit optimization under the three correlated noise models (depolarizing, amplitude damping, phase damping). Uses MMA optimizer to tune circuit parameters for maximum fidelity.

## Configuration

Edit these in `TOPAZ + noise.ipynb`:

```python
N_QUBITS = 8            # Number of qubits from 2 to n (but note: runtime grows exponentially)
MAX_ITERATIONS = 40     # Optimization iterations
DEPOL_BASE = 0.005      # Depolarizing noise rate
AMP_DAMP_BASE = 0.008   # Amplitude damping rate
PHASE_DAMP_BASE = 0.012 # Phase damping rate
VD_COPIES = 2           # Virtual distillation copies
```



## Output

**Example in text:**
```
Initial: Noisy Fidelity=0.172247, Ideal Fidelity=0.175201
Iter  1: NoisyFid=0.343567 (Δ=+0.1713)
...
Final Optimized Noisy Fidelity: 0.370883
Final VD Enhanced Fidelity: 0.378–0.385
Total Runtime: 476.44 seconds  # for 8 qubits
```

**Key Parameters:**

- ρ params (9): Operator intensity coefficients
- τ params (9): Temporal evolution angles
- Noise correlation: Exponential decay with distance
- Move limit: Adapts based on improvement

**Typical Performance:**

- Initial noisy fidelity: 7%–70%+
- Final optimized: 28%–96% (varies with random initialization)
- VD enhancement: +0.8%–7%
- Runtime (8 qubits): 1–3 hours

## Code Structure

| Class/Function | Purpose |
|---|---|
| `TopologyAwareUPTE` | Build unitary operators with which MMA can be applied directly |
| `MMAOptimizer` | Optimization engine |
| `apply_correlated_*_noise()` | Noise channels |
| `noise_aware_objective_function()` | Fidelity under noise |
| `reliable_hybrid_gradient()` | Gradient computation |
| `run_mma_optimization()` | Main loop |

## Troubleshooting

| Issue | Solution |
|---|---|
| **Import error** | `pip install qiskit` |
| **Too slow** | Reduce `N_QUBITS`, `MAX_ITERATIONS`, or `VD_COPIES` |
| **Different results** | Expected — random initialization causes improvements to vary with exponentially larger parameter spaces per additional qubit |
| **Reproduce exactly** | Set `np.random.seed(42)` |
