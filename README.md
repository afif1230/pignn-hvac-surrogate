# Physics-Informed MeshGraphNet Surrogate for Indoor Thermal Comfort and HVAC Optimization

Code and data accompanying the paper "A Physics-Informed MeshGraphNet Surrogate for Indoor Thermal Comfort Prediction and HVAC Optimization."

A MeshGraphNet surrogate is trained on steady-state CFD of a single-occupant office to predict the full indoor temperature, velocity, and radiation fields on the simulation mesh. The trained surrogate is then used as the forward model in a comfort-constrained HVAC setpoint optimization over an outdoor temperature record.

## Repository structure

- `data/pyg_dataset.pt` - PyG dataset (train/val/test graphs)
- `model/Model.ipynb` - architecture, weight loading, test-set evaluation, cross-section plots
- `model/model.pt` - trained model weights
- `model/crosssection_best.png`, `model/crosssection_worst.png` - best- and worst-case CFD vs surrogate cross-sections
- `optimization/Optimization.ipynb` - baseline comfort, lookup table, and optimizer
- `optimization/hourly_temperature.csv` - processed hourly outdoor temperature
- `optimization/rbc_baseline_hourly.csv` - rule-based baseline: setpoints, energy, volumetric PMV
- `optimization/gnn_lookup_table_v3.npz` - precomputed surrogate lookup table over the setpoint grid
- `optimization/optimized_hourly_v3.csv` - optimized hourly setpoints and energy
- `optimization/*.png` - baseline schedule, daily energy, and optimized trajectory figures

## Requirements

- Python 3.13
- torch
- torch_geometric
- pythermalcomfort
- numpy, pandas, scipy, matplotlib, tqdm

Install with:

    pip install torch torch_geometric pythermalcomfort numpy pandas scipy matplotlib tqdm

## Usage

Run each notebook from within its own folder, as the file paths are relative.

### Model evaluation (model/Model.ipynb)

Run top to bottom. It loads the trained weights and the dataset, evaluates the surrogate on the held-out test cases, and reproduces the reported accuracy (RMSE, MAE, NMAE, R-squared) for temperature, velocity, and radiation. It also produces the best- and worst-case cross-section figures.

### Optimization (optimization/Optimization.ipynb)

Run top to bottom. It loads the trained model and dataset from the model/ and data/ folders, evaluates the rule-based baseline comfort, builds the surrogate lookup table over the setpoint grid, and runs the hourly optimization, reporting the energy savings relative to the baseline.

## Data

The dataset and trained model are provided for evaluation. The raw CFD flow fields are available from the corresponding author on request.

## License

Released under the MIT License. See LICENSE.
