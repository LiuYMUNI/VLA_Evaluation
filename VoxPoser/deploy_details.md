# Deployment Details for VoxPoser

This document outlines the steps to get the VoxPoser codebase up and running.

## Getting Started

**Important Note:** It is highly recommended to run this codebase with a monitor. If you intend to run in headless mode, please refer to the specific instructions provided within the RLBench documentation.

## Environment Setup

1.  **Create a Conda Environment:**
    Open your terminal or command prompt and execute the following commands to create a new Conda environment named `voxposer-env` with Python 3.9:
    ```bash
    conda create -n voxposer-env python=3.9
    ```

2.  **Activate the Conda Environment:**
    After creation, activate the newly created environment:
    ```bash
    conda activate voxposer-env
    ```

## Installation

1.  **Install PyRep and RLBench:**
    Follow the instructions provided in the respective documentation for installing PyRep and RLBench. **Ensure that these are installed within the `voxposer-env` Conda environment you just created.**

2.  **Clone the VoxPoser Repository:**
    Clone the VoxPoser codebase from its GitHub repository:
    ```bash
    git clone https://github.com/huangwl18/VoxPoser.git
    ```

3.  **Install Additional Dependencies:**
    Navigate into the cloned `VoxPoser` directory and install the remaining dependencies using `pip`:
    ```bash
    cd VoxPoser
    pip install -r requirements.txt
    ```

4.  **OpenAI API Key:**
    Obtain an OpenAI API key. You will need to insert this key into the first cell of the demo notebook before running it.

## Running the Demo

1.  **Locate the Demo Code:**
    The main demonstration code is located in `src/playground.ipynb`.

2.  **Follow Notebook Instructions:**
    Open `src/playground.ipynb` (e.g., using Jupyter Notebook or JupyterLab) and follow the instructions provided within the notebook cells to run the demo.
