# -Lunar-Landing-Literature-Review
A curated collection of papers, open‑source tools, and datasets focused on **autonomous lunar landing** — spanning **Reinforcement Learning, Optical Navigation, Convex Optimization‑based Guidance, and Terrain‑Relative Navigation**.  ---


### Repository Structure
lunar-landing-review/
├── papers/ # Key paper PDFs and BibTeX citations
├── tools/ # Notes on open‑source tools (LuPNT, GMAT, Nyx, etc.)
├── datasets/ # Links to CRESENT‑365, LU5M812TGT, etc.
├── summary_tables/ # Markdown tables comparing methods
├── assets/ # Diagrams and flowcharts
└── README.md




---

### Quick Links

| Resource | Description | Link |
|----------|-------------|------|
| **CRESENT‑365** | First public dataset emulating a year‑long lunar mapping mission — 15,283 rendered images with SPICE‑derived Sun angles[reference:0] | [arXiv](https://arxiv.org/abs/2509.20748) |
| **LuPNT** | Open‑source C++/Python library for Lunar PNT research (Stanford NAV Lab)[reference:1] | [GitHub](https://github.com/Stanford-NavLab/LuPNT) |
| **Successive Convexification** | Trajectory optimizer for 6‑DoF powered descent guidance (Szmuk et al.)[reference:2] | [GitHub](https://github.com/BenChung/SuccessiveConvexification) |
| **CVX Soft‑Landing Optimizer** | Program to compute optimal soft‑landing solutions via lossless convexification[reference:3] | [GitHub](https://github.com/ravi4ram/Soft-Landing-Optimizer) |
| **PECAN** | Crater Identification by Perspective Cone Alignment[reference:4] | [GitHub](https://github.com/ckchng/PECAN) |
| **Nyx** | High‑fidelity astrodynamics toolkit (Rust) — contributed to 3 lunar missions[reference:5] | [nyxspace.com](https://nyxspace.com) |
| **GMAT** | NASA’s open‑source mission design & trajectory optimization[reference:6] | [software.nasa.gov](https://software.nasa.gov) |



2.1 Reinforcement Learning for Lunar Landing
RL has evolved from simple policy gradient methods to sophisticated meta‑RL and constrained RL frameworks that simultaneously handle guidance, navigation, control, and hazard avoidance.

2.1.1 Meta‑RL and Integrated GNC
The most recent trend integrates meta‑reinforcement learning into complete guidance‑navigation‑control (GNC) architectures.

Furfaro et al. (2025) — Meta‑Reinforcement Learning GNC for Autonomous Lunar Landing with Safe Site Selection

• A stabilized seeker guidance algorithm combined with an autonomous safe landing site selection system.
• The seeker tracks the designated site by adjusting elevation/azimuth angles to centre the site in the sensor FOV.
• Seeker angles, closing speed, and range are used to formulate a velocity field mapped directly to commanded thrust for four thrusters.
• The G&C policy is optimized via meta‑RL; the landing site is selected via CNN semantic segmentation on a hazard map built from DEMs and simulated onboard camera images.
• Key result: Robust to seeker lag, actuator lag/degradation; compatible with multiple divert manoeuvres during powered descent.

Izzo et al. (2024) — Optimality Principles in Spacecraft Neural Guidance and Control

• Review discussing training end‑to‑end neural architectures for interplanetary transfers, planetary landings, and proximity operations.
• Demonstrates that neural models successfully learn optimality principles (e.g., bang‑bang control structure, fuel‑optimal switching times).

Federici & Furfaro (2024) — Improving RL in Spacecraft G&C through Meta‑Learning: Comparison on Planetary Landing

• Systematic comparison of standard RL vs. meta‑RL for planetary landing.
• Meta‑learning significantly improves sample efficiency and robustness to domain shifts.

2.1.2 Safe RL via Constrained Optimization
Yang et al. (2025) — Safe RL Framework for Lunar Lander Control (航空学报)

• Formulates the landing task as a Semi‑Markov Decision Process (SMDP).
• Compresses historical trajectories into abstract SMDP state transition graphs.
• Identifies critical state‑action pairs via risk identification and real‑time intervention.
• Key result: Up to 22% improvement in mission success rate and 42% improvement in safety metrics when applied on top of pre‑trained DQN, Dueling DQN, and DDQN models — without requiring additional sensors.

Belmonte‑Baeza et al. (2025) — Constrained RL for Lunar Surface Operations

• Constrained RL for quadrupedal mobile manipulators on the lunar surface.
• Integrates collision avoidance, dynamic stability, and power efficiency as hard safety constraints.
• Achieves 4 cm positional accuracy and 8.1° orientation accuracy for end‑effector pose tracking.

2.1.3 Decomposed RL: Site Selection + Guidance
KAIST (2024) — RL‑Based Powered Descent and Landing for Planetary Exploration

• Decomposes the overall landing problem into landing site selection and landing guidance subproblems to reduce complexity.
• Site selection: image‑processing techniques on altitude sensor data → individual safety factor maps → optimization maximizing weighted sum.
• Guidance: curriculum learning with episode‑level reward (not step‑level) to handle sparse reward.
• Key insight: The decomposed approach ensures real‑time capability while maintaining landing precision.

Gaudet et al. (2020) — Deep RL for Safe Landing Site Selection with Concurrent Consideration of Divert Maneuvers

• Integrated framework simultaneously:
(a) identifies safe landing locations based on slope and roughness, and
(b) plans in‑flight divert manoeuvres.
• RL agent optimizes site selection and guidance concurrently at the system level.

2.1.4 Image‑Based End‑to‑End RL
Scorsoglio et al. (2021) — Image‑Based Deep Reinforcement Meta‑Learning for Autonomous Lunar Landing

• Image‑based RL meta‑learning for pinpoint powered descent.
• Handles uncertain dynamic parameters and actuator failure.
• Uses sequences of descent images as input directly to the policy.

Furfaro et al. (2020) — Safe Lunar Landing via Images: RL Meta‑Learning for Hazard Avoidance

• RL meta‑learning and hazard detection embedded into a single system.
• Derives optimal thrust command from sequences of images + radar altimeter data.
• HDA algorithm integrated with real‑time GNC.

2.1.5 Algorithm Comparison Studies
A 2024 benchmark compared DQN, Double DQN (DDQN), and Policy Gradient algorithms on the LunarLander‑v2 environment. Key findings: DDQN achieved the best stability (reduced overestimation bias), Policy Gradient exhibited smoother control but higher variance, and DQN remained the fastest to converge but occasionally unstable.

A broader 2024 survey on agent‑based deep learning for space landing missions examined RL algorithms, simulation environments, and evaluation metrics employed across diverse landing scenarios, concluding that hybrid approaches (RL + optimal control initialization) consistently outperform pure RL.



2.2 Optical Navigation (Visual‑Based Navigation, Crater Detection, TRN)
Optical navigation is central to pinpoint landing (sub‑100 m accuracy). The key sub‑topics are: (1) crater‑based absolute navigation (AbsNav), (2) feature‑tracking relative navigation (RelNav), and (3) Terrain Relative Navigation (TRN) that matches real‑time descent images to a pre‑loaded reference map.

2.2.1 Integrated AbsNav + RelNav Architectures
Politecnico di Milano (2024) — Integrated Optical TRN for Autonomous Lunar Landing

• Presents a fusion of Relative Navigation (Visual Odometry via ORB detector with adaptive threshold + subwindowing) and Absolute Navigation (craters, rilles, wrinkled ridges as landmarks, detected via YOLO).
• Novel method to retrieve crater rims from YOLO bounding boxes + robust matching strategy.
• AbsNav corrects RelNav drift; RelNav adds robustness when known landmarks are sparse.
• Evaluated under various illumination, pointing, and terrain conditions via numerical simulation with synthetic lunar calibrated images.

Ostrogovich et al. (2024) — A Dual‑Mode Approach for Vision‑Based Navigation in Lunar Landing (CVPR 2024 Workshop)

• Novel dual‑mode navigation that switches between crater‑based AbsNav and feature‑tracking RelNav.
• Performance: ~270 m accuracy (AbsNav mode), 27.4 m horizontal / 0.8 m vertical accuracy (RelNav mode).
• Tested on flight‑representative embedded hardware, demonstrating real‑time feasibility.

2.2.2 Flight‑Proven Systems: SLIM, Chang’e‑6
SLIM (JAXA, January 2024): The first precision lunar landing, achieving an evaluated accuracy of within 10 m (requirement was 100 m).

• Key technology: Vision‑Based Navigation (VBN) used operatively during descent across 7 regions.
• Onboard GN&C algorithm corrects large initial state dispersion while respecting thrust pointing constraints and avoiding subsurface flight during terminal descent.
• The VBN matched real‑time descent images against a pre‑loaded lunar map using crater detection + feature tracking.

Chang’e‑6 (CNSA, June 2024): High‑precision visual positioning on the lunar far side (South Pole–Aitken Basin).

• Descent imagery fused with orbital data to build an initial landing zone model.
• Orthophoto feature‑point matching and crater‑recognition matching validated through computer vision registration.
• Intelligent vision‑guided trajectory reconstruction framework demonstrated simultaneous landing site localization (~metre‑level) and geological characterization.

2.2.3 Crater Detection & Identification
Modern crater‑based navigation uses deep‑learning detectors:

Detector	Dataset	Key Performance	Source
Mask R‑CNN (Chang’e‑5)	Chang’e‑5 Landing Camera	0.701 IoU for ellipse regression; robust to off‑nadir views	
YOLOv8	Chandrayaan‑2 OHRC	Outperforms Mask R‑CNN in both accuracy and speed	
STELLA (Mask R‑CNN + descriptor‑less matching)	CRESENT‑365 (15,283 images)	Metre‑level position, sub‑degree attitude accuracy across varying illumination and viewing angles	
YOLO family (JAXA/NASA benchmark)	1.2M known craters (Blender‑rendered)	Evaluated on LS1046 Space CPU and Google Coral TPU for flight readiness	
LU5M812TGT	~5M craters ≥0.4 km	First comprehensive AI‑driven global lunar crater catalog	
PECAN (Crater Identification by Perspective Cone Alignment) provides an alternative geometry‑based approach for matching observed craters to a known catalogue rather than relying solely on ML descriptors.

2.2.4 Terrain Relative Navigation (TRN)
TRN matches real‑time descent imagery to a pre‑loaded reference map. Recent advances:

• SURF‑based TRN model for Earth‑independent spacecraft localization.
• Explainable Convolutional Networks using attention‑based YOLOv3 and attention‑Darknet53‑LSTM for crater detection and pose estimation during landing.
• Deep learning‑based end‑to‑end absolute positioning: multi‑stage, multi‑head neural network that directly regresses geospatial coordinates, expanding altitude range constraints.

2.3 Guidance Calculations (Optimal Control, Convex Optimization, Computational Methods)
The mathematical core of lunar landing guidance. The standard formulation: minimize fuel consumption (or maximise final mass) subject to non‑convex dynamics (thrust magnitude lower bound, nonlinear gravity, mass depletion) and state/control constraints.

2.3.1 Convex Optimization & Successive Convexification
The dominant paradigm for real‑time onboard guidance is (successive) convexification — approximating the non‑convex optimal control problem as a sequence of convex sub‑problems (typically Second‑Order Cone Programs, SOCPs) solvable in milliseconds.

Foundational work:

• Açıkmeşe & Ploen (2007): First convex programming approach to powered descent guidance (originally for Mars) — the seminal paper.
• Açıkmeşe et al. (2013): Lossless convexification of non‑convex control bound and pointing constraints — proved that the convex relaxation is exact under mild conditions.
• Szmuk et al. (2018–2019): Successive convexification (SCvx) for real‑time 6‑DoF powered descent with state‑triggered constraints — handles thrust bound, pointing, and glide‑slope constraints.

Recent advances (2024–2026):

• SCP for 6‑DoF PDG with Compound State‑Triggered Constraints (2025): Extends SCvx to continuous‑time satisfaction of compound logical specifications (e.g., “activate sensor X when altitude < Y AND horizontal distance < Z”).
• Convex Optimization‑Based 6‑DoF Guidance (2025): Novel two‑stage method — first solves fuel‑optimal 3‑DoF via convex optimization, then a convex MPC minimizes thrust direction deviation (instead of conventional attitude errors) for 6‑DoF alignment. Outperforms quaternion feedback control.
• Integrated Lander‑Propulsion‑GNC Framework (2026): Successive convexification unified with propulsion architecture (throttle‑ratio, dead‑zone, gimbal authority). Achieves sub‑50‑metre landing precision in Monte Carlo under realistic perturbations. Demonstrates the fundamental coupling between throttle‑ratio, pointing authority, and surface gravity.
• Fault‑Tolerant Convex Guidance (2025): Lossless convexification extended for actuator fault scenarios during lunar soft landing.
• CTICPG (Constant Thrust Adaptive Convex Programming Guidance): For unthrottled engines, iteratively solves the propellant‑optimal guidance problem.
• Neural‑Network‑Based Fuel‑Optimal PDG (2024): Uses neural networks to approximate the optimal solution, trading formal guarantees for inference speed.
• Chandrayaan‑3 Convex Decision Boundary (2024): Convex optimization technique to compute feasibility boundaries for the powered descent phase — used operationally for the successful Chandrayaan‑3 landing.
• Navigation‑Optimal Convex Guidance (2024): Convexifies the navigation error variance alongside the guidance objective, accounting for TRN accuracy degradation during descent.

| Tool | Description | Source |
|------|-------------|--------|
| **SuccessiveConvexification** (BenChung) | C++/MATLAB implementation of Szmuk et al.’s SCvx algorithm for 6‑DoF powered descent | GitHub |
| **Soft‑Landing‑Optimizer** (ravi4ram) | Python implementation of lossless convexification paper (Açıkmeşe et al.) | GitHub |
| **openscvx** | Python successive convexification with JAX backend — GPU‑accelerated | PyPI |
| **Convex MPC for Soft Landing** | Reproduces PDG algorithm within an MPC framework | GitHub |
| **GMAT** | NASA’s open‑source mission design & trajectory optimization supporting lunar regimes | NASA |
| **Nyx** | Rust‑based high‑fidelity astrodynamics toolkit — operational on 3 lunar missions | nyxspace.com |
| **LuPNT** | Stanford NAV Lab’s C++/Python simulator for lunar PNT research | GitHub |
2.4 Optical Flow‑Based Methods for Resource‑Constrained Landers
Cowan et al. (2025) — ESA Advanced Concepts Team + ispace:

• A motion‑field inversion framework using optical flow and rangefinder‑based depth estimation for egomotion during lunar descent.
• Sparse optical flow features extracted via pyramidal Lucas‑Kanade algorithm.
• Depth modelled as planar or spherical terrain parameterized by a laser rangefinder.
• Performance: Sub‑10% velocity error for complex south pole terrain, ~1% for typical terrain — on a CPU budget compatible with small landers.
• Represents a lightweight alternative to heavy crater‑detection pipelines.


| Detector | Dataset | Key Performance | Source |
|----------|---------|-----------------|--------|
| **Mask R‑CNN** (Chang’e‑5) | Chang’e‑5 Landing Camera | 0.701 IoU for ellipse regression; robust to off‑nadir views | |
| **YOLOv8** | Chandrayaan‑2 OHRC | Outperforms Mask R‑CNN in both accuracy and speed | |
| **STELLA** (Mask R‑CNN + descriptor‑less matching) | CRESENT‑365 (15,283 images) | Metre‑level position, sub‑degree attitude accuracy across varying illumination and viewing angles | |
| **YOLO family** (JAXA/NASA benchmark) | 1.2M known craters (Blender‑rendered) | Evaluated on LS1046 Space CPU and Google Coral TPU for flight readiness | |
| **LU5M812TGT** | ~5M craters ≥0.4 km | First comprehensive AI‑driven global lunar crater catalog | |



| Tool | Language | Key Capability |
|------|----------|----------------|
| **LuPNT** | C++/Python | Lunar PNT simulation (Stanford NAV Lab) |
| **GMAT** | C++/Python | Multi‑mission trajectory optimization, lunar regimes |
| **Nyx** | Rust/Python | High‑fidelity orbit propagation, estimation; 3 lunar missions |
| **Poliastro** | Python | Interactive astrodynamics library (MIT license) |
| **Orekit** | Java | Low‑level space dynamics library, ESA operational |
| **PSS Optical Navigation Module** | MATLAB | Optical navigation for SCT; orbit determination using horizon, craters, sun/stars |
| **LunarLandingTRNSim** | MATLAB | Optimal 2D trajectory → 3D north‑pole landing + optical navigation test |



| Dataset | Size | Description | Source |
|---------|------|-------------|--------|
| **CRESENT-365** | 15,283 images | Year‑long lunar mapping mission emulation; DEM‑rendered with SPICE | |
| **CRESENT+** | — | Extended crater‑annotated dataset for CBN training | |
| **LU5M812TGT** | ~5M craters ≥0.4 km | Global AI‑powered crater database with precise coordinates and dimensions | |
| **Chang’e‑5 Landing Camera** | — | Real lunar descent images with off‑nadir angles | |
| **Chandrayaan‑2 OHRC** | Medium‑large craters | Benchmark for YOLOv8 vs. Mask R‑CNN comparison | |
| **JAXA/NASA Blender Dataset** | 1.2M craters | Rendered with ground‑truth bounding boxes; flight‑hardware evaluated | |
| **NAC CDR Lunar Crater Dataset** | ~20,000 craters | Crater detection studies from LRO NAC images | GitHub |



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





