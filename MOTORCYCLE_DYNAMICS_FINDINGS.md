# Motorcycle Dynamics — findings for this repo (Jolt Physics engine)

Jolt's `Jolt/Physics/Vehicle/MotorcycleController` was assessed as a candidate for
engineering-grade motorcycle dynamics in the "Multibody Motorcycle Realistic
Dynamics" effort. The full benchmark chain (G1–G9) lives in **`mojoco-simulation`**,
branch `claude/motorcycle-dynamics-multibody-28pytq`, folder `benchmarks/`.

## Assessment

`MotorcycleController` / `VehicleConstraint` model a motorcycle as a **single rigid
body** with raycast/cast-cylinder wheels, kept upright by an explicit **lean
controller** (a PID spring: `mLeanSpringConstant/Damping/IntegrationCoefficient`).
This is ideal for games/VR (it is robust, cheap, and feels right), consistent with
Jolt's stated design goal of approximating rigid-body behaviour for games.

For *engineering-grade* single-track dynamics it is a dead-end, because the
property that defines real motorcycle stability — emergent self-balancing from the
**gyroscopic + trail coupling** of the spinning wheels — is replaced by the PID
lean controller rather than integrated. The Whipple–Carvallo benchmark (the
field's reference) requires that coupling; a general articulated-body integrator
(MuJoCo) reproduces it to machine precision and self-stabilises with no controller,
which is why the dynamics work was built there.

This is an evaluation note only — **no engine change is proposed or made**. Jolt
remains excellent for its intended purpose. See `mojoco-simulation/benchmarks/`
(README + REFERENCES) for the engineering-grade model and the full G1–G9 analysis.
