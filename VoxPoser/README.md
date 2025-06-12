# VoxPoser: Model Overview & Replication

**VoxPoser** is a vision-language-action system that leverages large language models (LLMs) and vision-language models (VLMs) to generate closed-loop robot manipulation trajectories from natural language instructions—**without relying on pre-defined motion primitives or additional training**.

## Core Methodology

- **LLMs for Reasoning and Planning:**  
  The system uses LLMs (such as GPT-family models) to infer task-relevant affordances and constraints from free-form language instructions. The LLM also generates Python code on the fly.

- **Grounding with VLMs:**  
  The LLM-generated code interacts with a VLM (like CLIP) to compose a sequence of **3D value maps** (affordance maps and constraint maps), which are spatially grounded in the robot’s observation space (from RGB-D input).

- **Trajectory Synthesis:**  
  The value maps are used as objective functions for a model-based motion planner, which generates a dense sequence of 6-DoF end-effector waypoints (robot arm poses) for complex manipulation tasks.

- **Zero-shot, Open-set Generalization:**  
  VoxPoser supports open-vocabulary language instructions and open-set object references, synthesizing trajectories for previously unseen tasks and objects with no additional training.

- **Online Adaptation:**  
  The framework can incorporate online experiences by learning a dynamics model for contact-rich scenes, increasing robustness to dynamic perturbations.

## VoxPoser Pipeline

Given an RGB-D scene observation and a natural language instruction:

1. **Instruction Parsing:**  
   The LLM parses the instruction, infers high-level task structure, and generates executable code.

2. **Affordance and Constraint Mapping:**  
   The code interacts with the VLM and scene data to generate value maps representing where and how the robot should act.

3. **Motion Planning:**  
   The value maps are passed to a planner, which synthesizes a trajectory of end-effector waypoints to achieve the instructed goal.

4. **(Optional) Online Learning:**  
   The system can adapt by learning local dynamics from experience in contact-rich tasks.

![VoxPoser Pipeline](../assets/voxposer_pipeline.png) <!-- Replace with your actual image filename -->

## Key Features

- **No Pre-defined Motion Primitives:**  
  All trajectories are synthesized from value maps and planners, enabling flexible and general manipulation.

- **No Additional Training Required:**  
  All reasoning and mapping are done zero-shot via LLM/VLM and code generation.

- **Rich Generalization:**  
  Can follow open-vocabulary instructions for novel objects and tasks in both simulation and real-robot experiments.

## Paper & Reference

- [VoxPoser: Composing 3D Value Maps for Robotic Manipulation with Language-Conditioned Code](https://arxiv.org/abs/2307.05973)

## Replication Details

See [`deploy_details.md`](./deploy_details.md) for step-by-step setup and deployment instructions.

## Results

See the [`results/`](./results/) folder for videos and experiment logs from my VoxPoser replications.

---
