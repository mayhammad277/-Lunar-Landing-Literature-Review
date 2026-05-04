# Lunar Landing Literature Review

A curated collection of papers, open‑source tools, and datasets focused on **autonomous lunar landing** — spanning **Reinforcement Learning, Optical Navigation, Convex Optimization‑based Guidance, and Terrain‑Relative Navigation**.

---

## Repository Structure


lunar-landing-review/
├── papers/ # Key paper PDFs and BibTeX citations
├── tools/ # Notes on open‑source tools (LuPNT, GMAT, Nyx, etc.)
├── datasets/ # Links to CRESENT‑365, LU5M812TGT, etc.
├── summary_tables/ # Markdown tables comparing methods
├── assets/ # Diagrams and flowcharts
└── README.md





---

## Quick Links

| Resource | Description | Link |
|----------|-------------|------|
| **CRESENT‑365** | First public dataset emulating a year‑long lunar mapping mission — 15,283 rendered images with SPICE‑derived Sun angles | [arXiv](https://arxiv.org/abs/2509.20748) |
| **LuPNT** | Open‑source C++/Python library for Lunar PNT research (Stanford NAV Lab) | [GitHub](https://github.com/Stanford-NavLab/LuPNT) |
| **Successive Convexification** | Trajectory optimizer for 6‑DoF powered descent guidance (Szmuk et al.) | [GitHub](https://github.com/BenChung/SuccessiveConvexification) |
| **CVX Soft‑Landing Optimizer** | Program to compute optimal soft‑landing solutions via lossless convexification | [GitHub](https://github.com/ravi4ram/Soft-Landing-Optimizer) |
| **PECAN** | Crater Identification by Perspective Cone Alignment | [GitHub](https://github.com/ckchng/PECAN) |
| **Nyx** | High‑fidelity astrodynamics toolkit (Rust) — contributed to 3 lunar missions | [nyxspace.com](https://nyxspace.com) |
| **GMAT** | NASA’s open‑source mission design & trajectory optimization | [software.nasa.gov](https://software.nasa.gov) |

---

## 1. Reinforcement Learning for Lunar Landing

RL has evolved from simple policy gradient methods to sophisticated meta‑RL and constrained RL frameworks that simultaneously handle guidance, navigation, control, and hazard avoidance.

### 1.1 Meta‑RL and Integrated GNC

The most recent trend integrates meta‑reinforcement learning into complete guidance‑navigation‑control (GNC) architectures.

| Year | Authors | Title / Summary | Key Contribution |
|------|---------|-----------------|------------------|
| 2025 | Furfaro et al. | Meta‑RL GNC for autonomous lunar landing with safe site selection | Seeker guidance + CNN site selection + meta‑RL thrust control — robust to actuator lag and multiple divert manoeuvres |
| 2024 | Izzo et al. | Optimality Principles in Spacecraft Neural Guidance and Control | Neural architectures learn optimal control structures (bang‑bang, switching times) |
| 2024 | Federici & Furfaro | Improving RL in Spacecraft G&C through Meta‑Learning | Meta‑learning improves sample efficiency and domain‑shift robustness for planetary landing |

### 1.2 Safe RL via Constrained Optimization

| Year | Authors | Title / Summary | Key Contribution |
|------|---------|-----------------|------------------|
| 2025 | Yang et al. | Safe RL Framework for Lunar Lander Control (SMDP‑based) | +22% success rate, +42% safety on top of DQN family, no extra sensors required |
| 2025 | Belmonte‑Baeza et al. | Constrained RL for Lunar Surface Operations | 4 cm positional accuracy, 8.1° orientation accuracy for quadrupedal manipulator; hard safety constraints |

### 1.3 Decomposed RL: Site Selection + Guidance

| Year | Authors | Title / Summary | Key Contribution |
|------|---------|-----------------|------------------|
| 2024 | KAIST | RL‑Based Powered Descent & Landing for Planetary Exploration | Decomposition: image‑based site selection + curriculum‑learning guidance; real‑time capable |
| 2020 | Gaudet et al. | Deep RL for Safe Landing Site Selection with Divert Maneuvers | Concurrent site selection and divert manoeuvre planning |

### 1.4 Image‑Based End‑to‑End RL

| Year | Authors | Title / Summary | Key Contribution |
|------|---------|-----------------|------------------|
| 2021 | Scorsoglio et al. | Image‑Based Deep RL Meta‑Learning for Autonomous Lunar Landing | Pinpoint powered descent from image sequences; handles uncertain dynamics and actuator failures |
| 2020 | Furfaro et al. | Safe Lunar Landing via Images: RL Meta‑Learning for Hazard Avoidance | End‑to‑end image‑to‑thrust policy with hazard detection; integrated HDA + GNC |

### 1.5 Algorithm Comparison Studies

*   **2024 benchmark** – DQN, Double DQN, Policy Gradient compared on LunarLander‑v2. DDQN most stable; Policy Gradient smoother but higher variance; DQN fastest convergence.
*   **2024 survey** on agent‑based deep learning for space landing missions concluded that **hybrid approaches (RL + optimal control initialization)** consistently outperform pure RL.

---

## 2. Optical Navigation (Visual‑Based Nav, Crater Detection, TRN)

Optical navigation is central to **pinpoint landing** (sub‑100 m accuracy). The key pillars are **(1) crater‑based absolute navigation (AbsNav)**, **(2) feature‑tracking relative navigation (RelNav)**, and **(3) Terrain Relative Navigation (TRN)**.

### 2.1 Integrated AbsNav + RelNav Architectures

| Year | Authors | Title / Summary | Key Contribution |
|------|---------|-----------------|------------------|
| 2024 | Politecnico di Milano | Integrated Optical TRN for Autonomous Lunar Landing | YOLO‑based crater/landmark detection + ORB visual odometry; robust under varying illumination and pointing |
| 2024 | Ostrogovich et al. | A Dual‑Mode Vision‑Based Navigation for Lunar Landing | CVPR Workshop; ~270 m (AbsNav), 27.4 m/0.8 m (RelNav); tested on flight‑representative embedded hardware |

### 2.2 Flight‑Proven Systems

| Mission | Year | Key Technology | Achieved Accuracy |
|---------|------|----------------|-------------------|
| **SLIM (JAXA)** | 2024 | Vision‑Based Navigation across 7 descent regions; crater detection + feature tracking | **within 10 m** (requirement 100 m) |
| **Chang’e‑6 (CNSA)** | 2024 | Orthophoto feature‑point matching + crater‑recognition on lunar far side | **~metre‑level** landing site localization |

### 2.3 Crater Detection & Identification

| Detector | Dataset | Key Performance | Source |
|----------|---------|-----------------|--------|
| **Mask R‑CNN (Chang’e‑5)** | Chang’e‑5 Landing Camera | 0.701 IoU for ellipse regression; robust to off‑nadir views | |
| **YOLOv8** | Chandrayaan‑2 OHRC | Outperforms Mask R‑CNN in both accuracy and speed | |
| **STELLA** (Mask R‑CNN + descriptor‑less matching) | CRESENT‑365 (15,283 images) | Metre‑level position, sub‑degree attitude accuracy across varying illumination | |
| **YOLO family (JAXA/NASA benchmark)** | 1.2M known craters (Blender‑rendered) | Evaluated on LS1046 Space CPU and Google Coral TPU | |
| **LU5M812TGT** | ~5M craters ≥0.4 km | First comprehensive AI‑driven global lunar crater catalog | |

**PECAN** (Crater Identification by Perspective Cone Alignment) provides a geometry‑based alternative to ML‑only descriptor matching.

### 2.4 Terrain Relative Navigation (TRN)

TRN matches real‑time descent imagery to a pre‑loaded reference map. Recent advances:

*   SURF‑based TRN model for Earth‑independent spacecraft localization.
*   Explainable Convolutional Networks using attention‑based YOLOv3 and attention‑Darknet53‑LSTM for crater detection and pose estimation.
*   Deep learning‑based end‑to‑end absolute positioning: multi‑stage, multi‑head neural network that directly regresses geospatial coordinates.

---

## 3. Guidance Calculations (Optimal Control, Convex Optimization)

The standard formulation: minimize fuel consumption subject to **non‑convex** dynamics (thrust magnitude lower bound, nonlinear gravity) and state/control constraints.

### 3.1 Foundational Work

| Year | Authors | Contribution |
|------|---------|--------------|
| 2007 | Açıkmeşe & Ploen | First convex programming approach to powered descent guidance (Mars) |
| 2013 | Açıkmeşe et al. | Lossless convexification of non‑convex control bounds — exact relaxation under mild conditions |
| 2018–2019 | Szmuk et al. | Successive convexification (SCvx) for real‑time 6‑DoF powered descent with state‑triggered constraints |

### 3.2 Recent Advances (2024–2026)

| Year | Approach / Title | Key Contribution |
|------|------------------|------------------|
| 2025 | SCP for 6‑DoF PDG with Compound State‑Triggered Constraints | Extends successive convexification to continuously satisfy logical combinations (e.g., “activate sensor X when altitude < Y **AND** horizontal distance < Z”) |
| 2025 | Convex Optimization‑Based 6‑DoF Guidance | Two‑stage method: (1) fuel‑optimal 3‑DoF solution via convex optimisation, (2) convex MPC that minimises **thrust‑direction deviation** (instead of attitude error) for 6‑DoF alignment; outperforms quaternion feedback |
| 2026 | Integrated Lander‑Propulsion‑GNC Framework | Unifies successive convexification with propulsion architecture (throttle ratio, dead‑zone, gimbal authority); achieves **sub‑50 m landing precision** in realistic Monte Carlo simulations |
| 2025 | Fault‑Tolerant Convex Guidance | Lossless convexification extended for actuator fault scenarios during lunar soft landing |
| – | CTICPG (Constant Thrust Adaptive Convex Programming Guidance) | Propellant‑optimal guidance for **unthrottled** engines |
| 2024 | Neural‑Network‑Based Fuel‑Optimal PDG | Neural network approximates optimal solution — trades formal guarantees for **inference speed** |
| 2024 | Chandrayaan‑3 Convex Decision Boundary | Convex optimisation to compute **feasibility boundaries** for powered descent; used operationally in Chandrayaan‑3 landing |
| 2024 | Navigation‑Optimal Convex Guidance | Convexifies **navigation error variance** together with guidance objective, explicitly accounting for TRN accuracy degradation |

### 3.3 Open‑Source Optimization Tools

| Tool | Description | Source |
|------|-------------|--------|
| **SuccessiveConvexification (BenChung)** | C++/MATLAB implementation of Szmuk et al.'s SCvx for 6‑DoF powered descent | GitHub |
| **Soft‑Landing‑Optimizer (ravi4ram)** | Python implementation of lossless convexification (Açıkmeşe et al.) | GitHub |
| **openscvx** | Python successive convexification with JAX backend — GPU‑accelerated | PyPI |
| **Convex MPC for Soft Landing** | Reproduces PDG algorithm within an MPC framework | GitHub |
| **GMAT** | NASA’s open‑source mission design & trajectory optimization supporting lunar regimes | NASA |
| **Nyx** | Rust‑based high‑fidelity astrodynamics toolkit — operational on 3 lunar missions | nyxspace.com |
| **LuPNT** | Stanford NAV Lab’s C++/Python simulator for lunar PNT research | GitHub |

---

## 4. Optical Flow‑Based Methods for Resource‑Constrained Landers

| Year | Authors | Title / Summary | Key Contribution |
|------|---------|-----------------|------------------|
| 2025 | Cowan et al. (ESA + ispace) | Vision‑Guided Optic Flow Navigation for Small Lunar Missions | Motion‑field inversion with pyramidal Lucas‑Kanade + rangefinder depth; **sub‑10% velocity error** for complex south‑pole terrain on a CPU budget |

---

## 5. Datasets

| Dataset | Size | Description | Source |
|---------|------|-------------|--------|
| **CRESENT‑365** | 15,283 images | Year‑long lunar mapping emulation; DEM‑rendered with SPICE‑derived Sun angles | |
| **CRESENT+** | – | Extended crater‑annotated dataset for CBN training | |
| **LU5M812TGT** | ~5M craters ≥0.4 km | Global AI‑powered crater database with precise coordinates and dimensions | |
| **Chang’e‑5 Landing Camera** | – | Real lunar descent images with off‑nadir angles | |
| **Chandrayaan‑2 OHRC** | Medium‑large craters | Benchmark for YOLOv8 vs. Mask R‑CNN comparison | |
| **JAXA/NASA Blender Dataset** | 1.2M craters | Rendered with ground‑truth bounding boxes; flight‑hardware evaluated | |
| **NAC CDR Lunar Crater Dataset** | ~20,000 craters | Crater detection studies from LRO NAC images | GitHub |

---

## 6. Simulation Frameworks & Libraries

| Tool | Language | Key Capability |
|------|----------|----------------|
| **LuPNT** | C++/Python | Lunar PNT simulation (Stanford NAV Lab) |
| **GMAT** | C++/Python | Multi‑mission trajectory optimization, lunar regimes |
| **Nyx** | Rust/Python | High‑fidelity orbit propagation, estimation; 3 lunar missions |
| **Poliastro** | Python | Interactive astrodynamics library (MIT license) |
| **Orekit** | Java | Low‑level space dynamics library, ESA operational |
| **PSS Optical Navigation Module** | MATLAB | Optical navigation for SCT; orbit determination using horizon, craters, sun/stars |
| **LunarLandingTRNSim** | MATLAB | Optimal 2D trajectory → 3D north‑pole landing + optical navigation test |

---

## 7. Key GitHub Repositories

| Repository | Description | Stars | Language |
|------------|-------------|-------|----------|
| [Stanford-NavLab/LuPNT](https://github.com/Stanford-NavLab/LuPNT) | Lunar PNT Simulator | – | C++/Python |
| [BenChung/SuccessiveConvexification](https://github.com/BenChung/SuccessiveConvexification) | Szmuk et al.'s SCvx optimizer | – | C++/MATLAB |
| [ravi4ram/Soft-Landing-Optimizer](https://github.com/ravi4ram/Soft-Landing-Optimizer) | Lossless convexification soft‑landing | – | Python |
| [ckchng/PECAN](https://github.com/ckchng/PECAN) | Crater identification via perspective cone alignment | – | Python |
| [y316284799/Optimizing-Spacecraft-Trajectories](https://github.com/y316284799/Optimizing-Spacecraft-Trajectories) | Convex optimization landing trajectories | – | MATLAB |
| [openscvx](https://pypi.org/project/openscvx/) | Successive convexification (JAX) | – | Python |
| [isatyamks/Crater-Detection-on-Lunar-Surface](https://github.com/isatyamks/Crater-Detection-on-Lunar-Surface) | AI/ML crater detection (OHRC) | – | Python |
| [Abeey04/Identification-of-safe-navigation-routes-using-Chandrayaan-Images](https://github.com/Abeey04/Identification-of-safe-navigation-routes-using-Chandrayaan-Images) | YOLOv5 + ACO pathfinding | – | Python |

---

## 8. Research Landscape – Key Papers at a Glance

| Year | Authors | Title | Venue | Key Contribution |
|------|---------|-------|-------|------------------|
| 2025 | Furfaro et al. | Meta‑RL GNC for autonomous lunar landing with safe site selection | Neural Comput. & Applic. | Seeker guidance + CNN site selection + meta‑RL thrust control — robust to actuator degradation |
| 2025 | McLeod et al. | AI‑Enabled Crater‑Based Navigation for Lunar Mapping (STELLA) | arXiv | First end‑to‑end CBN pipeline for mapping; CRESENT‑365 dataset; metre‑level accuracy across all illumination |
| 2025 | Yang et al. | Safe RL framework for lunar lander control (SMDP‑based) | 航空学报 | +22% success rate, +42% safety on top of DQN family |
| 2025 | Cowan et al. | Vision‑Guided Optic Flow Navigation for Small Lunar Missions | arXiv | Motion‑field inversion; CPU‑only; sub‑10% vel. error — lightweight alternative |
| 2025 | Aklan et al. | Integrated Lander‑Propulsion‑GNC Framework for Autonomous Lunar PD | IEEE RAST 2026 | SCvx + propulsion coupling; sub‑50‑m landing precision |
| 2025 | Kyushu Univ. | Convex Optimization‑Based 6‑DoF Guidance for Lunar PD | IFAC ACA 2025 | Convex MPC for thrust‑direction alignment; outperforms quaternion feedback |
| 2024 | Ostrogovich et al. | Dual‑Mode Vision‑Based Navigation for Lunar Landing | CVPR Workshop | ~270 m (AbsNav), 27.4 m/0.8 m (RelNav); flight hardware tested |
| 2024 | KAIST | RL‑Based Powered Descent & Landing for Planetary Exploration | Master’s thesis | Decomposition: site selection + guidance; curriculum learning episode reward |
| 2024 | JAXA/NASA | ML‑Based Crater Detection for TRN | AAS GN&C Conf. | Rendering tool for training data; YOLO family on flight hardware |
| 2024 | Izzo et al. | Optimality Principles in Spacecraft Neural G&C | Science Robotics | Neural architectures learn optimality principles (bang‑bang, switching) |
| 2024 | SLIM Team | Vision‑Based Navigation Flight Results in SLIM Lunar Landing | Acta Astronautica | Operational VBN across 7 regions; pinpoint landing confirmed within 10 m |
| 2024 | Chang’e‑6 Team | Descent Image Navigation & Positioning Technology | 航天学报 | Orthophoto feature matching + crater recognition for far‑side landing validation |

---

## 9. Future Directions & Open Problems

1. **RL‑Convex Optimization Hybrids** – Using RL to warm‑start convex solvers or learn the terminal cost‑to‑go in SCvx, combining safety guarantees with adaptability.
2. **Unified CBN for Full Mission Profiles** – Extending crater‑based navigation from powered descent to year‑long mapping campaigns and orbital regimes.
3. **Certifiable Safety for Learned Controllers** – Applying Control Barrier Functions (CBFs) or reachability analysis to RL‑based landing policies.
4. **Multi‑Sensor Fusion with Learned Models** – Integrating optical flow, craters, inertial, altimetry, and GNSS‑R with learned fusion that adapts to varying data quality.
5. **Edge Deployment** – Moving from GPU‑bound architectures to flight‑ready inference on radiation‑tolerant processors (LS1046, Google Coral TPU).
6. **Adversarial Robustness** – Understanding how visual navigation degrades under off‑nominal illumination, terrain, and camera artefacts.
7. **Explainable & Verifiable AI for Space** – Interpretable crater detection and pose estimation networks verifiable per ECSS‑E‑ST‑40C / DO‑178C standards.

---

*Last updated: 2026‑05‑03. Contributions welcome!*
