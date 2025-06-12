# RTandOcto/README.md

## 🤖 RT-1 and Octo Deployment in SimplerEnv

This folder contains my experiments testing two prominent vision-language-action (VLA) models — **RT-1** and **Octo** — inside the [SimplerEnv](https://github.com/wenzhengwang/simpler-env) simulation environment. These trials demonstrate basic deployment, action prediction, and robot interaction for VLA models in simulation.

### 🧪 What’s Included

* **`RTandOcto_in_SimplerEnv.ipynb`**: Unified notebook to load and test RT-1 and Octo in SimplerEnv.
* **Configuration & instructions**: The notebook contains setup details and model loading procedures.

### 📌 About the Models

#### RT-1: Robotic Transformer

* Developed by Google Robotics.
* Trained on 130k+ episodes of real-world data from a robot fleet.
* Predicts actions from images and instructions using a transformer-based policy.

#### Octo

* An open VLA model with multi-task robotic capabilities.
* Designed to generalize over different manipulation tasks and environments.
* Shares architectural similarities with RT-1 but often exhibits broader generalization capacity.

### 🧰 Usage

* Launch Jupyter and run the notebook `RTandOcto_in_SimplerEnv.ipynb`.
* Switch models inside the notebook by toggling cell execution for RT-1 vs. Octo.
* Observe and visualize robot behavior with respect to text instructions.

### 📁 Folder Structure

```
RTandOcto/
├── RTandOcto_in_SimplerEnv.ipynb   # Main test notebook
└── README.md                               # This file
```

---

This module complements other VLA deployments in this repo (OpenVLA, Pi0, VoxPoser) and reflects hands-on experience with instruction-conditioned robot control.
