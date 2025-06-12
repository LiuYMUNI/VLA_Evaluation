# Pi0 Deployment in ALOHA Environment

This guide details the deployment of the Pi0 policy using the OpenPI framework on a local machine in the ALOHA simulated environment.



## 1. System Requirements

- **OS:** Ubuntu 22.04
- **GPU Driver:** Install appropriate NVIDIA drivers: [https://www.nvidia.com/drivers/lookup/](https://www.nvidia.com/drivers/lookup/)
- **Anaconda:** Install from [Anaconda archive](https://repo.anaconda.com/archive/Anaconda3-2024.10-1-Linux-x86_64.sh)
- **Model Checkpoint:** Download from [ModelScope](https://www.modelscope.cn/models/lerobot/pi0/files)



## 2. Pi0 Deployment

### 2.1 Framework Selection
There are two major frameworks for deploying Pi0:

- [OpenPI (official)](https://github.com/Physical-Intelligence/openpi)
- [Lerobot](https://github.com/huggingface/lerobot)

**Note:** This guide uses OpenPI. OpenPI is based on Google's Flax framework and designed for high-speed inference via JAX + CUDA. Flax is lighter-weight and faster compared to PyTorch/TensorFlow-based frameworks.



### 2.2 Create Environment
```bash
# Install Git
sudo apt install git

# Create conda environment
conda create -n pi0 python=3.11
conda activate pi0

```



### 2.3 Install Inference Services

```bash
# Create directory for installation
sudo mkdir /home/robot
cd /home/robot

# Clone lerobot (needed for some dependencies)
git clone https://github.com/huggingface/lerobot.git
cd lerobot
git checkout 6674e368249472c91382eb54bb8501c94c7f0c56
pip install -e .

# Return to /home/robot root directory
cd ..

# Clone and install openpi
git clone https://github.com/Physical-Intelligence/openpi.git
cd openpi/packages/openpi-client
pip install -e .

# Use uv if needed for dependency resolution
GIT_LFS_SKIP_SMUDGE=1 uv pip install -e .
```



### 2.4 Start Inference Server (Example)

```bash
python scripts/serve_policy.py \
    policy:checkpoint \
    --policy.config=pi0_fast_droid \
    --policy.dir=s3://openpi-assets/checkpoints/pi0_fast_droid
```

> 📌 This is just an example to check missing packages. If missing packages are encountered, refer to `pyproject.toml` and `uv.lock` to determine the exact versions required and install them manually. 




### 2.5 Client Testing
- Edit `examples/simple_client/main.py`, set host to `127.0.1.1`, default port is `8000`.
- In a new terminal:
```bash
cd openpi
python examples/simple_client/main.py --env=DROID
```
> If successful, the deployment is now complete!



### 2.6 Simulation Testing
In **Terminal Window 1**:
```bash
uv run scripts/serve_policy.py --env ALOHA_SIM
```
In **Terminal Window 2**:
```bash
MUJOCO_GL=egl python examples/aloha_sim/main.py
```
If you encounter EGL-related errors:
```bash
sudo apt-get install -y libegl1-mesa-dev libgles2-mesa-dev
```



## 3. Notes and Troubleshooting (Q&A)

1. **Dependency Conflicts During Installation**  
   Check `uv.lock` and manually install version-matching packages.

2. **Slow Inference or Timeout Errors**  
   Likely due to missing GPU drivers. Reinstall correct drivers from NVIDIA.

3. **CUDA Device Mismatch Errors**  
   Example: “built for device CUDA:0 (RTX 4090); cannot run on device CUDA:3 (RTX 3090)”  
   Fix: Match driver to actual GPU and reinstall.

4. **Inference Server Inaccessible**  
   Disable firewall or open relevant ports for inference service.


