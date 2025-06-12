# Pi0 Deployment on LIBERO

This guide walks you through deploying Pi0 on the LIBERO simulated environment using the official OpenPI repository.



## 🔧 1. Setup Instructions

### 1.1 Clone the Repo

```bash
git clone --recurse-submodules git@github.com:Physical-Intelligence/openpi.git
```

> If you encounter permission errors:
> - Make sure your SSH key is added to your GitHub account.
> - Add it at: https://github.com/settings/ssh/new

### 1.2 Create Environment and Install Dependencies

```bash
conda create -n uv_envs python=3.10
conda activate uv_envs
pip install uv
cd openpi/
GIT_LFS_SKIP_SMUDGE=1 uv sync
```

> `GIT_LFS_SKIP_SMUDGE=1` skips downloading large files during cloning. Use `git lfs pull` later to fetch them.



## 🚀 2. Launch the Server

### 2.1 Activate the Environment

```bash
source ./.venv/bin/activate
```

### 2.2 Start the Policy Server

```bash
uv run scripts/serve_policy.py --env LIBERO
```

> If you want to speed up startup, copy cached models from another user:
>
```bash
sudo cp -r /mnt/home/user/.cache/openpi /mnt/home/user/.cache/openpi
sudo chown user:user -R /mnt/home/user/.cache/openpi
```

> If HuggingFace fails to connect, a VPN/proxy may be required.



## 🧪 3. LIBERO Simulation Environment

### 3.1 Create Libero Virtual Environment

```bash
cd openpi
uv venv --python 3.8 examples/libero/.venv
source examples/libero/.venv/bin/activate
```

### 3.2 Install Dependencies

```bash
uv pip sync examples/libero/requirements.txt third_party/libero/requirements.txt \
  --extra-index-url https://download.pytorch.org/whl/cu113 \
  --index-strategy=unsafe-best-match

uv pip install -e packages/openpi-client
uv pip install -e third_party/libero
```

> ⚠️ Proxy may be required to complete installation.

### 3.3 Set Python Path

```bash
export PYTHONPATH=$PYTHONPATH:$PWD/third_party/libero
```



## ▶️ 4. Run Simulation

```bash
python examples/libero/main.py
```

- This will download the dataset and launch the Pi0 policy in simulation.
- You should observe trajectory execution in the LIBERO simulated tasks.



## ❓ FAQ & Troubleshooting

1. **Dependency Conflicts During Sync**
   - Manually match versions based on `uv.lock`.

2. **Slow or Timeout Inference**
   - Ensure CUDA drivers are installed correctly.

3. **GPU Device Mismatch Errors**
   - Install proper GPU driver matching your hardware.

4. **Server Inaccessible**
   - Open firewall ports or disable firewalls that block port 8000.



## 📎 References

- [OpenPI Official Blog](https://www.physicalintelligence.company/blog/openpi)
- [GitHub Repo](https://github.com/Physical-Intelligence/openpi)
- [HuggingFace Model](https://huggingface.co/lerobot/pi0/tree/main)
