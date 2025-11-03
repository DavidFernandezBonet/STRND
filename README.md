
# STRND

STRND = **S**patio **T**opological **R**ecovery by **N**etwork **D**iscovery


* Feed an edge list (pandas DataFrame, NX, PyG, list of edges)
* Get back node coordinates in 2D or 3D (pandas DataFrame)



## Install

```bash
pip install strnd
```


## Minimal use

```python
from strnd import get_strnd_positions_df

coords_df = get_strnd_positions_df(edge_df, dim=2)
# coords_df columns: node_ID, x, y
```

`edge_df` must have columns `source`, `target` (and optionally `weight`).

## Example with Graph Generation + Quality Metrics

```python

import numpy as np
from proxigraph.config import GraphConfig
from proxigraph.core import ProximityGraph
from PointQuality import QualityMetrics
from strnd import get_strnd_positions_df

np.random.seed(42)

# 1) create a graph
config = GraphConfig(dim=2, num_points=1000, L=1, point_mode="circle", proximity_mode="delaunay_corrected")
pg = ProximityGraph(config=config)
positions = pg.generate_positions()
edge_df = pg.get_edge_list(as_dataframe=True)

# 2) run STRND
rec_positions = get_strnd_positions_df(edge_df, dim=2)

# 3) quality
rec_positions_ordered = rec_positions.sort_values("node_ID")[["x","y"]].to_numpy()
qm = QualityMetrics(positions, rec_positions_ordered)
print(qm.evaluate_metrics(compute_distortion=False))

# 4) print head
print(rec_positions.head())
```

## dependencies

- numpy
- pandas
- matplotlib
- pecanpy (node2vec)
- umap-learn
- (optional) proxigraph for the demo

## Citation

If you use STRND in your work, please cite:

## Citation

If you use STRND in your work, please cite:

**Core STRND paper**

Fernández Bonet, D., & Hoffecker, I. T. (2023). *Image recovery from unknown network mechanisms for DNA sequencing-based microscopy.* Nanoscale, 15, 8153–8157. https://doi.org/10.1039/D2NR05435C

**Related work**

Dahlberg, S. K., Fernández Bonet, D., & Hoffecker, I. T. (2025). *Hidden network preserved in Slide-tags data allows reference-free spatial reconstruction.* Nature Communications. https://doi.org/10.1038/s41467-025-65295-w

Fernández Bonet, D., Blumenthal, J. I., Lang, S., Dahlberg, S. K., & Hoffecker, I. T. (2024). *Spatial coherence of DNA barcode networks.* bioRxiv. https://doi.org/10.1101/2024.05.12.593725

