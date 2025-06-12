# OpenVLA Deployment Guide

This guide outlines how to set up and run OpenVLA in two simulation environments:
- **Libero**: Officially supported and recommended
- **SimplerEnv**: Lightweight and minimal setup (for demo/testing)


## 🔧 Environment Setup

### 1. Create and Activate Conda Environment

```bash
conda create -n openvla python=3.10 -y
conda activate openvla
```

### 2. Install PyTorch (Update CUDA version if needed)

```bash
conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia -y
```
### 3. Clone Repositories

```bash
git clone https://github.com/openvla/openvla.git
cd openvla
pip install -e .
```

### 4. Download Model

```bash
cd /mnt/home/models
git clone https://huggingface.co/openvla/openvla-7b
# or use ModelScope:
# pip install modelscope
# modelscope download --model zixiaoBios/openvla-7b
```

### 5. Install FlashAttention 2

```bash
pip install packaging ninja
ninja --version  # should return 0
MAX_JOBS=4 pip install "flash-attn==2.5.5" --no-build-isolation
```


## ✅ Test Basic Inference

```bash
from transformers import AutoModelForVision2Seq, AutoProcessor
from PIL import Image
import torch


path = '/mnt/home/models/openvla-7b'
image_path = '/mnt/home/data/demo.png'

# Load Processor & VLA
processor = AutoProcessor.from_pretrained(path, trust_remote_code=True)
vla = AutoModelForVision2Seq.from_pretrained(
path,
attn_implementation="flash_attention_2", # [Optional] Requires
`flash_attn`
torch_dtype=torch.bfloat16,
low_cpu_mem_usage=True,
trust_remote_code=True
).to("cuda:0")
image: Image.Image = Image.open(image_path)
prompt = "In: What action should the robot take to {<INSTRUCTION>}?\nOut:"
# Predict Action (7-DoF; un-normalize for BridgeData V2)
inputs = processor(prompt, image).to("cuda:0", dtype=torch.bfloat16)
action = vla.predict_action(**inputs, unnorm_key="bridge_orig",
do_sample=False)
# Execute...
print(action)

```

## 🧪 Deploy on Libero (Recommended)

### 1. Install Libero

```bash
git clone https://github.com/Lifelong-Robot-Learning/LIBERO.git
cd LIBERO
pip install -e .

```

### 2. Install Libero Requirements

```bash
cd ../openvla
pip install -r experiments/robot/libero/libero_requirements.txt

```
### 3. (Optional) Download Finetuned Model

```bash
modelscope download --model zixiaoBios/openvla-7b-finetuned-libero-spatial1

```

### 4. Run Evaluation

```bash
python experiments/robot/libero/run_libero_eval.py \
    --model_family openvla \
    --pretrained_checkpoint /mnt/home/youzeshun/models/openvla-7b-finetuned-libero-spatial \
    --task_suite_name libero_spatial \
    --center_crop True

```


## 🧪 Deploy on SimplerEnv (Alternative)

### 1. Clone SimplerEnv

```bash
git clone https://gitee.com/hawking_you/SimplerEnv.git
git clone https://gitee.com/hawking_you/ManiSkill2_real2sim.git
mv ManiSkill2_real2sim SimplerEnv/

```
#### 📌 Make sure SimplerEnv and OpenVLA share the same Python environment.

### 2. Launch Demo in Jupyter Notebook

```bash
pip install jupyter
jupyter notebook --no-browser --port 8080 --ip=0.0.0.0

```
### 3. Run openvla_sim.ipynb

This notebook demonstrates how to load OpenVLA, interact with SimplerEnv, and visualize robot actions.
You can run different tasks like:
- google_robot_pick_coke_can
- google_robot_open_drawer
- widowx_stack_cube, etc.


## 🧠 Notes on OpenVLA-Env Interaction
- Input: image + instruction → Output: 7-DoF action vector.
- Actions control translation, rotation, and gripper state.
- Works in a closed-loop with the simulator.
- In practice, OpenVLA can be deployed as an API endpoint for decoupled use.


## 🐞 Common Errors & Fixes
- FlashAttention installation hangs: use MAX_JOBS=4 pip install ...
- FlashAttention not found: check the install docs or try pip cache remove flash_attn
- CUDA Out of Memory: check your GPU memory and reduce batch size
- Numpy dtype mismatch: reinstall numpy to match expected version
- Undefined symbols: confirm torch, transformers, and flash_attn versions


