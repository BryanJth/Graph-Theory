# Graph Theory — Minimum Spanning Tree (MST) for Safer Aid Distribution Routes (DKI Jakarta)

This project models **DKI Jakarta sub-districts (kecamatan)** as a **weighted graph** and applies a **Minimum Spanning Tree (MST)** (via **Kruskal’s algorithm**) to propose a **“safer” cross-district route map** for **social aid (bansos) distribution** during COVID-19, where “safer” means **minimizing estimated exposure risk**.

Deliverables included:
- **Slides**: `Graph Presentation.pdf`
- **Poster**: `Graph Poster.pdf`
- **Notebook**: `Tegraf (1).ipynb`

---

## TL;DR (Main Takeaways)
- DKI Jakarta can be represented as a graph with **42 vertices (kecamatan)** and **90 edges (adjacency connections)**.
- Each kecamatan is assigned a **COVID-19 infection exposure risk** (node weight), derived from population + COVID case counts.
- Each route between adjacent kecamatan is assigned an **edge weight** using a simple and interpretable rule:  
  **edge risk = average(node risk A, node risk B)**.
- Applying **MST (Kruskal)** gives a connected “route network” with the **minimum total risk** under the defined weighting scheme.
- Example: traveling from **Pasar Rebo → Kramat Jati** is recommended to go via **Ciracas** on the MST path.

---

## Results / Output Summary

### Graph scale
- **Vertices:** 42 kecamatan  
- **Edges:** 90 adjacency edges  

Output includes:
- Weighted graph visualization (risk on edges)
- MST result graph (minimum total risk network)
- Example path queries on the MST

### Quick “risk” snapshot (pessimistic weights)
Highest-risk kecamatan (top 5):
- **Cempaka Putih (1.57)**
- **Gambir (1.22)**
- **Menteng (1.16)**
- **SetiaBudi (1.14)**
- **Senen (1.12)**

Lowest-risk kecamatan (bottom 5):
- **Kalideres (0.33)**
- **Jagakarsa (0.44)**
- **Cakung (0.47)**
- **Ciracas (0.50)**
- **Cilincing (0.52)**

> Note: values above reflect the project’s **pessimistic node weights** (confirmed positives only), as used for the main graph/MST.

---

## Problem Statement
1. How do we model **DKI Jakarta** (kecamatan-level) as a graph?
2. How do we assign **risk-based weights** to districts and routes?
3. How can **MST** help propose a **minimum-risk** connected route map?

---

## Methodology

### 1) Graph construction
- Each **kecamatan** is a **vertex**.
- An **edge** exists if two kecamatan are **adjacent** (share boundary).

### 2) Weighting (risk as “cost”)
This project uses exposure-risk framing: a major portion of risk is attributed to **encounters with infected individuals**, so the key parameter becomes the **probability of meeting infected people**.

**Node weights (kecamatan risk):**
- Computed from:
  - COVID case counts (Nov 2020)
  - Population per kecamatan (Dec 2020)
- Two versions were prepared:
  - **Pessimistic weight**: confirmed positive cases only
  - **Optimistic weight**: confirmed positive + suspected cases  
- The **graph + MST uses the pessimistic weights** (more “certain” by confirmed data).

**Edge weights (route risk between kecamatan):**
- The problem needs risk per **route**, not only per **district**.
- A simple approach is used:
  - **edge_weight(u, v) = (risk(u) + risk(v)) / 2**
- Rationale: bansos distribution covers broad areas, and we focus on a route map that minimizes risk under the assumptions used.

### 3) Minimum Spanning Tree (Kruskal)
- Apply **Kruskal’s algorithm** to get MST:
  - Select smallest-weight edges
  - Avoid cycles
  - Continue until all vertices are connected
- MST provides a connected network with **minimum total edge weight** (minimum total risk).

---

## Interpretation Example (From Slides)
If a distributor starts at **Pasar Rebo** and wants to reach **Kramat Jati**, the MST-recommended path is:
- **Pasar Rebo → Ciracas → Kramat Jati**

---

## Dataset / Data Sources
Used for the weighting computation:
- **Population** by kecamatan (BPS DKI Jakarta)
- **COVID-19 cases** by kecamatan (Nov 2020; Satu Data / COVID dataset)

> The notebook also includes hard-coded lists for nodes/edges/weights used in the final graph and MST visualization.

---

## Repository Structure
```text
Graph-Theory/
├─ Graph Poster.pdf
├─ Graph Presentation.pdf
└─ Tegraf (1).ipynb
```

---

## How to Run

### Option A — Run the notebook (recommended)
1) Create environment + install dependencies
```bash
pip install numpy pandas matplotlib seaborn networkx openpyxl
```

2) Open and run
- Open `Tegraf (1).ipynb` with Jupyter Notebook / JupyterLab / VSCode
- Run cells in order to reproduce:
  - weighted graph visualization
  - MST (Kruskal) result visualization
  - example path interpretation

### Option B — Minimal run notes
- If you don’t need the Excel import cells, you can still run the core graph/MST sections because the notebook includes the necessary **node/edge lists** for the final graph construction.
- If you want to recompute weights from your own raw tables, update the data-loading section and regenerate node risks.

---

## Assumptions / Limitations
- “Safe route” is defined **only** by the chosen risk weighting (COVID exposure risk proxy).
- Other real-world factors are not modeled, such as:
  - traffic, distance, travel time
  - logistics constraints, road accessibility
  - PPE usage, delivery protocol, crowd control
- Risk is treated as a static snapshot (based on the chosen month and data availability).

---

## References
- Badan Pusat Statistika Provinsi DKI Jakarta. (2021). *Provinsi DKI Jakarta dalam Angka*.  
- Chartrand, G., & Zhang, P. (2012). *A First Course in Graph Theory*. Dover Publications.  
- Smyth, B. (2021). *Estimating Exposure Risk to Guide Behaviour During the SARS-CoV-2 Pandemic* (Brief Research Report).  
- Satu Data (COVID-19 dataset): https://katalog.satudata.go.id/dataset/?tags=covid19&res_format=CSV

---

## Acknowledgements
Created as part of an academic exercise on **graph theory** and **applied MST (Kruskal)** for a real-world motivated routing problem.
