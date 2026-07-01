# In Silico Gas-Phase Protein Unfolding

<img width="4312" height="1200" alt="unfolding" src="https://github.com/user-attachments/assets/6ebe6269-0d10-46f8-be25-8d8040d4688b" />

This repository contains a computational pipeline to simulate gas-phase protein unfolding, serving as an *in silico* proxy for Collision-Induced Unfolding (CIU) mass spectrometry. The core simulation is executed via GROMACS, and the trajectory is rendered interactively in the browser using NGLView.

> **[View Simulation notebook on GitHub Pages](https://kssrikar4.github.io/In-Silico-Gas-Phase-Protein-Unfolding/)**

## Methodology & Architecture

Experimental CIU deposits kinetic energy via gas collisions, causing proteins to unfold. Simulating individual gas-atom impacts is computationally inefficient due to vast empty space and tiny collision cross-sections. 

Instead, this pipeline uses a **thermodynamic simulation as proxy**:
1. **Vacuum Preparation:** The protein is stripped of solvent and assigned a specific gas-phase charge state.
2. **Oversized Box Trick:** Modern GROMACS (using Verlet neighbor lists) strictly requires Periodic Boundary Conditions (PBC). To simulate a true vacuum without periodic image artifacts, so the protein is embed in a massively oversized box (10.0 nm padding) and use a plain Coulomb cut-off (5.0 nm) that is strictly smaller than half the box dimension.
3. **Temperature Ramp:** An NVT simulated annealing protocol linearly ramps the temperature (e.g., 300 K to 1000 K), distributing internal vibrational energy to trigger unfolding.

*Note: Standard aqueous force fields (like AMBER/CHARMM) over-stabilize salt bridges in a vacuum. For publishable research use gas-phase optimized force fields.*

## Prerequisites

To run the simulation locally, you need:
- **GROMACS**
- **Python 3.8+**
- **Jupyter Notebook / JupyterLab**
- **Python Libraries:** `MDAnalysis`, `nglview`, `ipywidgets`

Install the Python dependencies via:
```bash
pip install MDAnalysis nglview ipywidgets
```

## Usage

### 1. Run Locally (Jupyter Notebook)
1. Place your starting structure in the repository root and name it `protein.pdb`.
2. Open `Simulate.ipynb` in Jupyter.
3. Execute the cells sequentially. The notebook will:
   - Generate the GROMACS topology and vacuum box.
   - Write the MDP files for energy minimization and the temperature ramp.
   - Execute the GROMACS simulation via CLI.
   - Render an interactive 3D widget of the unfolding trajectory.

### 2. View Online (GitHub Pages)
The notebook is exported to index.html and hosted via [GitHub Pages](https://kssrikar4.github.io/In-Silico-Gas-Phase-Protein-Unfolding) for zero-install viewing.

*Note: The web version contains pre-computed trajectory data. It cannot execute the GROMACS backend.*

## Acknowledgements

This project relies on the foundational work of several open-source scientific communities:
- **GROMACS Development Team:** For providing a highly optimized, GPU-accelerated molecular dynamics engine.
- **MDAnalysis & NGLView Developers:** For building the Python ecosystem that bridges heavy MD trajectory parsing with high-performance WebGL rendering.
- **Qwen AI:** For assistance, code generation, and debugging.

## License

Distributed under the **GNU Lesser General Public License v3.0 (LGPL-3.0)**. 

See the [LICENSE](LICENSE) file for the full legal text.
