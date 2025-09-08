# Content of each Course

## Lecture 1 

<details>

<summary> PPT 1&2 </summary>

**Paradigm**: Perceive - Think - Act (Example P23 - PPT2)

**System Design**: Hardware + Software
- Hardware = Sensors + Actuators
- Software = State Estimation + Planning + Control(CUD + ROS)

**Sensors**: Camera + Lidar + Radar + IMU + Torque Sensors + Strain Gauge Pressure Sensor

> The limitation of lidar: finite angles to detect

**Robot vision** is embodied, active and environmentally situated.

</details>

## Lecture 2

<details>

<summary> PPT 3&4 </summary>

### PPT 3

#### 1. Course Agenda (L3 focus)

* Objectives: understand the breadth of vision tasks and datasets relevant to robot AI.&#x20;
* Major blocks:

  * Image analysis (classification, detection, segmentation)
  * Video understanding (tracking, trajectory prediction, action recognition/detection)
  * Special views for embodied systems: **egocentric** and **bird-eye-view (BEV)** perception
  * Misc: VQA, synthesis, audio-visual tasks

---

#### 2. Image Analysis

##### 2.1 Image Classification

* Problem statement (input tensor `[H,W,3]` → class probabilities).
* Typical datasets: MNIST, CIFAR-10/100, ImageNet, JFT.&#x20;
* Methods recap:

  * CNN family: AlexNet, VGG, ResNet, MobileNet, EfficientNet.
  * Transformer-based: ViT, Swin (patch embedding + attention).&#x20;

##### 2.2 Object Detection

* Problem statement: predict bounding boxes (+ optional class masks).
* Key datasets: PASCAL VOC, MS COCO, OpenImages.&#x20;
* Methods (high-level taxonomy):

  * **Two-stage methods** (proposal → classify & refine).
  * **One-stage methods** (dense/anchor or anchor-free prediction in one pass).&#x20;
* Evaluation metrics: AP / mAP (COCO-style: AP@\[.5:.95], AP50, AP75), per-size AP (small/medium/large).
* Practical notes: class imbalance, NMS, focal loss, anchor design.

##### 2.3 Segmentation

* Semantic segmentation (per-pixel class): Cityscapes, ADE20K; example method: DeepLab.&#x20;
* Instance / panoptic segmentation: Mask R-CNN extensions.

---

#### 3. Video Understanding

##### 3.1 Object Tracking

* Multi-Object Tracking (MOT): detection + data association (ID assignment).
* Multi-Target Multi-Camera (MTMC) tracking & ReID (city-scale scenarios, AI City).&#x20;

##### 3.2 Trajectory Prediction

* Predict future trajectories of agents (people/vehicles).
* Problem framed as: given past positions → predict future trajectory distribution.

##### 3.3 Action Recognition & Detection

* Action recognition (video-level classification): datasets UCF-101, Kinetics, Moments-in-Time; methods I3D, SlowFast, transformer variants.&#x20;
* Temporal action localization / action detection: locate start/end times (THUMOS, ActivityNet) and spatio-temporal boxes (AVA, VIRAT, MEVA).&#x20;

---

#### 4. Egocentric (First-Person) Perception

* Definition & importance for robots/AR (head-mounted cameras, hand–object interactions).
* Major datasets:

  * **Ego4D** — 3,000+ hours of first-person video; tasks: forecasting, episodic memory, hands and objects.&#x20;
  * **Ego-Exo4D** — paired first- and third-person data for demonstration learning.&#x20;
* Common tasks: hand/object detection, egocentric action anticipation, replay/episodic QA.

---

#### 5. Bird-Eye-View (BEV) Perception

* Motivation: unify multi-camera / multi-sensor inputs into top-down (map-like) representation for driving/robot navigation.&#x20;
* High-level BEV pipeline options:

  * **3D→2D**: project 3D points into camera planes → sample 2D features.
  * **2D→3D**: predict per-pixel depth / distribution → lift 2D features to 3D voxels → collapse to BEV.&#x20;
* Sensor fusion: RGB + LiDAR (late fusion vs joint fusion).&#x20;
* BEV datasets: KITTI / KITTI-360, SemanticKITTI, nuScenes, Waymo Open Dataset, Argo.&#x20;

---

### PPT 4

#### 1. Camera Model Basics

* **Pinhole model**: 3D point projects to 2D plane.
* **Thin lens model**: approximate real cameras.
* **Homogeneous coordinates**: unify rotation, translation, projection in linear form.

---

#### 2. Perspective Projection (3 key steps)

1. **Camera frame → image plane**

   * $p = [X_c/Z_c, Y_c/Z_c]$.
   * Encodes perspective (division by depth).
2. **Image plane → pixel coordinates**

   * Use intrinsic matrix $K$.
   * $[u,v,1]^T = K [x,y,1]^T$.
   * Accounts for focal length in pixels, principal point, skew.
3. **World frame → camera frame**

   * Apply extrinsics $[R|t]$.
   * $P_c = R P_w + t$.
   * Aligns world points with camera center & axes.

---

#### 3. Intrinsics & Extrinsics

* **Intrinsics (K):**

  * $f_x, f_y$: focal length in pixels
  * $(c_x, c_y)$: principal point
  * skew $s$ (usually 0)
* **Extrinsics (R, t):**

  * Rotation & translation from world to camera frame.

---

#### 4. Homogeneous Representation

* Projection equation:

  $$
  s \begin{bmatrix} u\\v\\1 \end{bmatrix} = K [R|t] \begin{bmatrix} X_w\\Y_w\\Z_w\\1 \end{bmatrix}
  $$
* Benefits:

  * Linear composition of transforms.
  * Enables DLT (linear estimation of P).
  * Handles points at infinity, projective geometry.

---

#### 5. Camera Calibration

* **Goal:** estimate intrinsics & extrinsics.
* **Need ≥ 6 correspondences** between 3D world points ↔ 2D image points.
* **Tools:** checkerboard pattern, OpenCV calibration toolbox.
* **Steps:**

  1. Detect 2D corners (pixel coords).
  2. Know 3D corner positions in world coords.
  3. Solve for $K, R, t$ using reprojection error minimization.

</details>
