# TriGaze: Camera-Guided 3D Representations for Robust In-Vehicle Gaze Estimation

This repository serves as the official supplementary material page for the paper **"TriGaze: Camera-Guided 3D Representations for Robust In-Vehicle Gaze Estimation"** (Accepted to IEEE ICIP 2026). 

Due to the strict page limits of the conference proceedings, we provide extended experimental results and technical clarifications here. These evaluations directly address the cross-domain generalization capability, parameter efficiency, and the core mechanism of our proposed framework.

---

## 1. Cross-Domain Generalization (Gaze360 Benchmark)

While TriGaze is primarily designed and optimized for challenging in-vehicle environments, we explicitly validate its broader generalization by evaluating on the unconstrained **Gaze360** benchmark. This introduces a significant domain shift, rigorously testing robustness against diverse backgrounds, varying illumination, and a 360° gaze range.

Without modifying our core geometric architecture, TriGaze achieved a highly competitive mean angular error (MAE) of **10.60°** on the full test set. 

| Method | Venue | Mean Angular Error (MAE) ↓ |
| :--- | :---: | :---: |
| Pinball LSTM | ICCV 2019 | 13.50° |
| GazePTR | CVPR 2024 | 13.45° |
| GazeLLE | CVPR 2025 | 11.81° |
| CA-Net | AAAI 2020 | 11.20° |
| GazeTR-Conv | ICPR 2022 | 11.09° |
| GazeDPTR | CVPR 2024 | 10.75° |
| **TriGaze (Ours)** | **ICIP 2026** | **10.60°** |

These steady improvements over both foundational and the latest cutting-edge methods prove that the observed performance leap stems from genuine, robust modeling of inherent 3D facial geometry. It allows the model to effectively adapt across completely different physical environments and extreme viewpoints.

---

## 2. Efficiency & Model Capacity

To prove that performance gains stem from genuine geometric modeling rather than merely increased capacity, we compare our parameter efficiency against foundation-model-based approaches:

*   **TriGaze (Ours):** ~32.75M parameters $\rightarrow$ MAE **6.55°** (on IVGaze)
*   **GazeLLE (DINOv2-based):** >300M parameters $\rightarrow$ MAE **8.21°** (on IVGaze)

This ~10x efficiency gap definitively rules out model capacity as the primary driver of our performance. It demonstrates that massive backbones like DINOv2 are suboptimal for gaze tasks without the explicit geometric constraints we propose.

---

## 3. The Necessity of Geometric Modeling (Ablation)

Our core technical innovation is the **Camera-Parameter-Guided Feature Modulation (CFM)** module. Unlike generic 3D methods, CFM reformulates gaze estimation as a camera-conditioned feature query. By dynamically injecting camera intrinsics and extrinsics to re-weight tri-plane features, it explicitly rectifies extreme perspective distortions and occlusion noise unique to off-axis, in-vehicle environments.

Our ablation studies confirm the indispensability of this design:
*   **Removing the Tri-plane:** Error increases to 6.66°.
*   **Removing Camera Guidance (CFM):** Without camera conditioning, the model suffers severe degradation under mask occlusions (error increases to 7.37°).

This directly confirms that our camera-guided 3D modeling, independent of parameter volume, is the fundamental source of improvement and robustness.
