# VacationPlannerGeneticAlgorithm Documentation

Welcome! This project contains a simple genetic algorithm prototype for vacation planning. It demonstrates how different cost-related parameters (lodging, sightseeing, transport, and food) interact under a budget using a basic fitness function and mutation strategies.

## What this project includes
- A Jupyter notebook: `Genetic Algorithm.ipynb`
- Two public functions used throughout the notebook:
  - `individual(...)` — computes a combined cost/score for a candidate solution
  - `fitness(...)` — computes how good a candidate is relative to a `moneyTarget`
- Example genetic algorithm loops with different mutation intensities (2%, 4%, 5%)

## Quick start
- Open `Genetic Algorithm.ipynb` in Jupyter (e.g., VS Code, JupyterLab, or classic Jupyter Notebook).
- Run the cells from top to bottom to see the algorithm iterate and plot fitness across generations.

### Dependencies
- Python 3.8+
- `pandas`, `seaborn`

You can install requirements quickly:
```bash
pip install pandas seaborn
```

## Public API
- See the API reference for function signatures, parameters, and returns:
  - [API Reference](./api.md)

## Usage guides and examples
- End-to-end examples for running the genetic algorithm loops and plotting results:
  - [Usage Guide](./usage.md)

## Notes
- The notebook demonstrates three mutation rates (approx. 2%, 4%, 5%). You can tweak the mutation ranges (`random.uniform(...)`) to experiment with convergence behavior.
- This repository currently exposes functions inside the notebook. If you plan to use them from a Python module, consider refactoring the functions into a `.py` file and importing them in your scripts.