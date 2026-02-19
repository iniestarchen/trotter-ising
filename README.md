# Trotterized Transverse-Field Ising Model

> **Category**: optimization &nbsp;|&nbsp; **Difficulty**: advanced &nbsp;|&nbsp; **Qubits**: 4 &nbsp;|&nbsp; **Gates**: 19 &nbsp;|&nbsp; **Depth**: 9

Quantum simulation of the 1D transverse-field Ising model H = -J Σ ZᵢZᵢ₊₁ - h Σ Xᵢ. The first-order Trotter-Suzuki decomposition approximates e^{-iHt} ≈ (e^{iJdtZZ})^⊗ · (e^{ihdtX})^⊗ for small dt. The ZZ coupling is implemented via CNOT-Rz-CNOT; the X field via Rx rotations. Higher accuracy requires more Trotter steps (smaller dt) or higher-order formulas.

## Expected Output

State approximating e^{-iHt}|++++⟩ after one Trotter step

## Circuit

The OpenQASM 2.0 circuit is in [`circuit.qasm`](./circuit.qasm).

```
OPENQASM 2.0;
include "qelib1.inc";
// First-order Trotter step: H = -J*ZZ - h*X, J=1, h=0.5, dt=0.1
qreg q[4];
creg c[4];
// Initial state: |++++>
h q[0]; h q[1]; h q[2]; h q[3];
// ZZ coupling terms: exp(i*J*dt*ZZ) via CNOT-Rz-CNOT  (2*J*dt = 0.2)
cx q[0],q[1]; rz(0.2) q[1]; cx q[0],q[1];
cx q[1],q[2]; rz(0.2) q[2]; cx q[1],q[2];
cx q[2],q[3]; rz(0.2) q[3]; cx q[2],q[3];
// Transverse field: exp(i*h*dt*X) = Rx(2*h*dt) = Rx(0.1)
rx(0.1) q[0]; rx(0.1) q[1]; rx(0.1) q[2]; rx(0.1) q[3];
measure q[0] -> c[0];
measure q[1] -> c[1];
measure q[2] -> c[2];
measure q[3] -> c[3];
```

## Tags

`trotter` `ising` `quantum-simulation` `variational` `many-body`

## References

- [Lloyd, S. (1996). Universal Quantum Simulators. Science 273, 1073](https://doi.org/10.1126/science.273.5278.1073)

## License

MIT — part of the [OpenQC Algorithm Catalog](https://github.com/openqc-algorithms).
