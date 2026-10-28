# Earthquake Network Analysis (1995–2023)

A graph-theoretic study of global earthquakes between 1995 and 2023. Each earthquake is modeled as a node in a geographic network, and edges connect nearby events weighted by the inverse of the distance between them. Centrality and community-detection algorithms are then used to ask whether network structure reveals anything about where and how earthquakes cluster.

Built with **NetworkX** for the graph modeling and **Gephi** for visualization. Data from a public [Kaggle earthquake dataset](https://www.kaggle.com/).

---

## Research Question

Is there a relationship between a node's network-centrality measures and the physical attributes of the earthquake it represents? And can network analysis surface meaningful geological patterns — recurring hotspots, stable communities — across nearly three decades of events?

---

## Method

**Data preparation** (`notebooks/data preparation.ipynb`)
- Dropped sparse/irrelevant columns (`continent`, `country`, `location`).
- Mapped categorical fields (`alert`, `magType`) to numeric values for statistical analysis.
- Rounded coordinates to reduce computational complexity, then saved a clean `normalized_data.csv`.

**Network construction** (`notebooks/earthquake network analysis.ipynb`)
- Split the dataset into four periods of roughly equal event counts: **1995–2002, 2003–2010, 2011–2017, 2018–2023**.
- Built one undirected graph per period. Nodes carry the earthquake's full attributes (magnitude, time, geological parameters).
- Edges connect events within 1000 km, using the **Haversine** formula; edge weight is the inverse of the distance, so closer events are weighted more heavily.
- Exported each graph to GraphML for Gephi visualization (`notebooks/gephi_exports/`).

**Analysis**
- Node-degree distribution across periods.
- Four centrality measures: Degree, Closeness, Betweenness, and Eigenvector centrality.
- **Spearman** correlation between centrality measures and raw earthquake attributes (run for the top n = 5 and n = 10 nodes).
- Geographic mapping of the most central nodes onto a world map.
- Community detection comparing **Louvain**, **Greedy Modularity**, and **Girvan-Newman** by modularity score.

---

## Key Findings

- **Centrality measures are almost perfectly correlated with one another** — a node that scores high on degree centrality also scores high on closeness, betweenness, and eigenvector centrality.
- **Magnitude does not drive centrality.** The strongest correlate of centrality was distance-to-nearest-center (`dmin`) and geographic proximity to active zones, not how powerful the earthquake was.
- **Central nodes concentrate in the South Pacific** in every period, aligning tightly with the **Ring of Fire**. Across the four periods, **86.9% / 92.7% / 90.6% / 91.8%** of all events fell inside the Ring of Fire.
- **Community structure is stable over time.** Louvain gave the best modularity in every period and produced a near-constant number of communities (40–46), with no dramatic shifts across the 28 years.

---

## Repository Layout

```
data/                 raw and normalized earthquake datasets (CSV)
notebooks/            data preparation + network analysis (Jupyter)
notebooks/gephi_exports/   per-period graphs (GraphML)
ne_110m_admin_0_countries/ Natural Earth country shapefile (basemap)
report/               full written report + Gephi visualizations (PNG)
```

---

## Running

The notebooks run in the cloud without a local install, or locally with:

```bash
pip install networkx pandas scipy matplotlib geopandas
jupyter notebook
```

Open `notebooks/data preparation.ipynb` first to regenerate `normalized_data.csv`, then `notebooks/earthquake network analysis.ipynb` for the full analysis.

---

> Originally produced as a networks-and-algorithms course project.
