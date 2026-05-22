# The Role of Connection Density in an Adaptive Network with Chaotic Units

This repository contains source code, simulation data, and analysis notebooks for the study:

**"The Role of Connection Density in an Adaptive Network with Chaotic Units"**

We investigate how the average degree of connectivity, used here as a density parameter, shapes the global dynamics and structural properties of adaptive networks composed of chaotic units. The model is inspired by chaotic map networks introduced by Kaneko and by subsequent adaptive extensions such as Gong and van Leeuwen, with emphasis on small-world structure, clustering, node loss, and modular organization.

---

## Repository Structure

```text
src/                    Source code modules
  functions.py          Network and community metric computation
  model.py              Kaneko adaptive network model implementation
  utils.py              Utility functions for saving outputs

results/                Processed simulation data used by the notebooks
  results_p*/           Results organized by connection probability p
    simulation_*/       Independent realizations
      networks/         Adjacency matrices for metric and figure analysis
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

## How to Run the Simulations

1. Clone the repository:

```bash
git clone https://github.com/ramirop2021/The-Role-of-Connection-Density-in-an-Adaptive-Network-with-Chaotic-Units.git
cd The-Role-of-Connection-Density-in-an-Adaptive-Network-with-Chaotic-Units
```

2. Create a virtual environment, optional but recommended:

```bash
python -m venv venv
source venv/bin/activate      # Linux/macOS
venv\Scripts\activate.bat     # Windows
```

3. Install dependencies:

```bash
pip install -r requirements.txt
```

4. Run the main simulation script:

```bash
python main.py
```

New simulation outputs are written to folders named `results_p*/`. The analysis notebook expects the processed manuscript data under `results/results_p*/`, matching the structure included in this repository.

---

## Analysis Notebook

The notebook `analysis - 2026.ipynb` reproduces the figures currently used in the manuscript. It loads stored adjacency matrices, node states, and metric files from `results/`, and writes manuscript figures to `plots_paper/`.

The current figure outputs are organized as:

```text
plots_paper/fig1/       Community-level temporal metrics
plots_paper/fig2/       Clustering, path length, and small-world index
plots_paper/fig3/       Degree distributions
plots_paper/fig4/       Node and edge betweenness distributions
plots_paper/fig5/       Louvain community snapshots across densities
plots_paper/fig6/       Louvain snapshots for the representative intermediate-density case
plots_paper/fig7/       Louvain snapshots for the representative high-density case
```

All notebooks assume they are executed from the root of the repository, because paths to `results/` and `plots_paper/` are relative paths.

```bash
jupyter notebook "analysis - 2026.ipynb"
```

A working LaTeX installation is required to render the paper-style figures, since the plotting cells use Matplotlib with `usetex=True`.

### Notation Note

The code and output files keep the legacy variable name `omega` for the small-world index. In the manuscript, the same quantity is denoted as `\sigma`, following the conventional notation:

```text
sigma = (C / C_rand) / (L / L_rand)
```

Thus, the variable `omega` in `network_metrics.txt` corresponds to `sigma` in the paper.

---

## Dependencies

The Python dependencies are listed in `requirements.txt`:

- numpy
- pandas
- networkx
- matplotlib
- numba
- python-louvain

Jupyter is needed to run the notebook interactively, and LaTeX is needed for the final paper-style rendering of figures.

---

## Citation

If you use this code or the data provided in this work, please cite the associated research paper:

Ramiro Pluss, Pablo Martin Gleiser.  
"The Role of Connection Density in an Adaptive Network with Chaotic Units."  
*arXiv preprint* [arXiv:2505.11437](https://arxiv.org/abs/2505.11437)

---

## Contact

Ramiro Pluss  
Email: rpluss@itba.edu.ar  
LinkedIn: [https://www.linkedin.com/in/ramiropluss/](https://www.linkedin.com/in/ramiropluss/)  
GitHub: [https://github.com/ramirop2021](https://github.com/ramirop2021)

---

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.