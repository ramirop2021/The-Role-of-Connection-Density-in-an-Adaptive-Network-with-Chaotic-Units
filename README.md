# The Role of Connection Density in an Adaptive Network with Chaotic Units

This repository contains the source code, simulation data, and analysis notebooks associated with the study:

**“The Role of Connection Density in an Adaptive Network with Chaotic Units”**

We investigate how the initial average degree, used here as a connection-density parameter, shapes the structural organization and associated dynamical patterns of adaptive networks composed of chaotic units. The model is based on networks of coupled chaotic maps and on the adaptive rewiring mechanism introduced by Gong and van Leeuwen.

The analysis focuses on how connection density and coupling strength jointly influence clustering, small-world-like organization, community structure, effective node loss, degree heterogeneity, and shortest-path load.

---

## Repository Structure

```text
src/                    Source code modules
  functions.py          Network and community metric computation
  model.py              Adaptive chaotic-network model implementation
  utils.py              Utility functions for saving simulation outputs

results/                Processed simulation data used by the analysis notebook
  results_p*/           Results organized by connection probability p
    simulation_*/       Independent realizations
      networks/         Adjacency matrices used for metrics and figures
      states/           Node-state snapshots

plots_paper/            Figures generated for the manuscript
analysis - 2026.ipynb   Main notebook used to generate the current paper figures
main.py                 Entry point for running new simulations
config.json             Simulation configuration
requirements.txt        Python dependencies
README.md               Project documentation
LICENSE                 MIT license
```

---

## Installation

Clone the repository:

```bash
git clone https://github.com/ramirop2021/The-Role-of-Connection-Density-in-an-Adaptive-Network-with-Chaotic-Units.git
cd The-Role-of-Connection-Density-in-an-Adaptive-Network-with-Chaotic-Units
```

Create a virtual environment. This step is optional but recommended:

```bash
python -m venv venv
```

Activate the environment:

### Linux or macOS

```bash
source venv/bin/activate
```

### Windows

```bash
venv\Scripts\activate.bat
```

Install the required dependencies:

```bash
pip install -r requirements.txt
```

---

## Simulation Configuration

Simulation parameters are defined in `config.json`.

The default configuration corresponds to the manuscript parameters for \(p=0.06\), which gives an expected initial mean degree

\[
\langle k\rangle=p(N-1)\approx18.
\]

The default configuration is:

```json
{
    "N": 300,
    "T": 800,
    "a": 1.7,
    "p": 0.06,
    "epsilon": [0.10, 0.15, 0.20, 0.25, 0.30, 0.35, 0.36, 0.37, 0.40, 0.45, 0.50],
    "time_steps": 300000,
    "num_simulations": 10,
    "options": {
        "save_matrix": true,
        "save_states": true
    }
}
```

The parameters are:

- `N`: initial number of nodes.
- `T`: number of chaotic-map iterations between consecutive rewiring attempts.
- `a`: chaotic-map parameter.
- `p`: Erdős–Rényi connection probability.
- `epsilon`: coupling-strength values.
- `time_steps`: number of adaptive time steps.
- `num_simulations`: number of independent realizations for each parameter combination.
- `save_matrix`: whether adjacency-matrix snapshots are saved.
- `save_states`: whether node-state snapshots are saved.

---

## Reproducing the Manuscript Simulations

The manuscript considers four initial connection probabilities:

| Connection probability \(p\) | Expected mean degree \(\langle k\rangle=p(N-1)\) |
|---:|---:|
| 0.03 | 8.97 \(\approx 9\) |
| 0.06 | 17.94 \(\approx 18\) |
| 0.12 | 35.88 \(\approx 36\) |
| 0.18 | 53.82 \(\approx 54\) |

To reproduce the complete density sweep, run the simulations separately for:

```text
p = 0.03
p = 0.06
p = 0.12
p = 0.18
```

For each run, modify only the value of `p` in `config.json`, while keeping the remaining manuscript parameters unchanged:

```text
N = 300
T = 800
a = 1.7
time_steps = 300000
num_simulations = 10
epsilon = [0.10, 0.15, 0.20, 0.25, 0.30, 0.35, 0.36, 0.37, 0.40, 0.45, 0.50]
```

Run the simulation with:

```bash
python main.py
```

New simulation outputs are written to folders named according to the connection probability:

```text
results_p003/
results_p006/
results_p012/
results_p018/
```

Each output folder contains separate directories for the independent realizations and their corresponding adjacency matrices, node states, and network metrics.

Because random seeds were not fixed or recorded, rerunning the simulations generates independent realizations of the same computational protocol rather than exact replicas of individual stochastic trajectories.

---

## Stored Manuscript Data

The processed simulation outputs used to generate the manuscript figures are included under:

```text
results/
```

The analysis notebook expects the manuscript data to follow the structure:

```text
results/
  results_p003/
  results_p006/
  results_p012/
  results_p018/
```

Each density folder contains independent realizations and their saved network and state data.

The stored outputs allow the reported analyses and figures to be regenerated without rerunning the complete simulation sweep.

---

## Analysis Notebook

The notebook:

```text
analysis - 2026.ipynb
```

contains the analysis pipeline used to generate the current manuscript figures.

It loads stored adjacency matrices, node states, and network metrics from `results/`, and writes the resulting figures to:

```text
plots_paper/
```

Run the notebook from the root directory of the repository:

```bash
jupyter notebook "analysis - 2026.ipynb"
```

All notebook paths are relative to the repository root.

A working LaTeX installation is required to render the paper-style figures because the plotting cells use Matplotlib with:

```python
usetex=True
```

---

## Manuscript Figures

The current figure outputs are organized as:

```text
plots_paper/fig1/       Community-level temporal metrics
plots_paper/fig2/       Clustering, path length, and small-world index
plots_paper/fig3/       Degree distributions
plots_paper/fig4/       Node and edge betweenness distributions
plots_paper/fig5/       Community structure and node-state snapshots
plots_paper/fig6/       Representative intermediate-density case
plots_paper/fig7/       Representative highest-density case
```

The analyses include:

- Effective network size \(N(t)/N\).
- Number of detected communities.
- Normalized size of the largest community.
- Global clustering coefficient.
- Average shortest path length.
- Small-world index.
- Degree distributions.
- Node and edge betweenness distributions.
- Louvain community partitions.
- Representative structural and dynamical snapshots.

---

## Small-World Index Notation

The source code and output files retain the legacy variable name:

```text
omega
```

for the small-world index.

In the manuscript, the same quantity is denoted by \(\sigma\), following the conventional notation:

```text
sigma = (C / C_rand) / (L / L_rand)
```

Therefore, the variable `omega` in files such as:

```text
network_metrics.txt
```

corresponds to \(\sigma\) in the manuscript.

---

## Treatment of Isolated Nodes

Adaptive rewiring may generate nodes with degree zero, particularly at low connection densities.

For an isolated node, the normalized coupling term in the dynamical equation is undefined because \(M_i=0\). Since the original formulation does not specify an alternative dynamical rule for isolated units, degree-zero nodes are removed from the active network.

The quantity:

```text
N(t) / N
```

represents the fraction of active nodes remaining at adaptive time step \(t\), where \(N\) is the initial network size.

The complementary quantity:

```text
1 - N(t) / N
```

represents the cumulative fraction of nodes that have become isolated and been removed.

---

## Dependencies

The Python dependencies are listed in `requirements.txt`.

The main dependencies are:

- NumPy
- pandas
- NetworkX
- Matplotlib
- Numba
- python-louvain

Jupyter is required to run the analysis notebook interactively.

A LaTeX installation is required for the final paper-style rendering of figures.

---

## Reproducibility Notes

The repository supports two complementary levels of reproducibility.

### Figure reproduction

The stored outputs under `results/` can be loaded by `analysis - 2026.ipynb` to regenerate the figures reported in the manuscript.

### Simulation replication

The complete computational protocol can be repeated by running `main.py` separately for the four connection probabilities used in the manuscript.

Because the simulation includes stochastic initial network generation, random node-state initialization, stochastic rewiring-node selection, and community detection, newly generated outputs will not reproduce individual trajectories exactly when random seeds are not fixed. They instead provide independent realizations under the same model parameters and simulation protocol.

---

## Citation

If you use the code or data provided in this repository, please cite the associated research paper:

Ramiro Pluss and Pablo Martin Gleiser.  
**“The Role of Connection Density in an Adaptive Network with Chaotic Units.”**  
*arXiv preprint*, arXiv:2505.11437.

```bibtex
@article{pluss2025connection,
  title   = {The Role of Connection Density in an Adaptive Network with Chaotic Units},
  author  = {Pluss, Ramiro and Gleiser, Pablo Martin},
  journal = {arXiv preprint arXiv:2505.11437},
  year    = {2025}
}
```

---

## Contact

**Ramiro Pluss**

- Email: rpluss@itba.edu.ar
- LinkedIn: [https://www.linkedin.com/in/ramiropluss/](https://www.linkedin.com/in/ramiropluss/)
- GitHub: [https://github.com/ramirop2021](https://github.com/ramirop2021)

---

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
