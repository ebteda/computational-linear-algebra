# Computational Linear Algebra

Two projects from the course **Computational Linear Algebra for Large Scale Problems** at **Politecnico di Torino** (academic year 2025/2026).

---

## The two projects

| Project | What it does | Main tools |
|---|---|---|
| [PageRank](pagerank/) | Ranks web pages by importance, the way early Google did. Written from scratch. | NumPy |
| [PCA and k-Means](pca-clustering/) | Turns a survey of 93 questions into 5 simple "profiles", then finds groups of people. | pandas, scikit-learn, matplotlib |

Each folder has its own README with the idea, the results and how to run it.

---

## The idea in short

**PageRank:** a page is important if important pages link to it. This turns into a linear algebra problem: find one special vector (an eigenvector) of a big matrix.

**PCA and k-Means:** a survey has too many questions to look at one by one. PCA squeezes them into a few main directions. k-Means then finds groups of people who answer in a similar way.

---

## Folders

```
computational-linear-algebra/
├── pagerank/
│   ├── 01_paper_examples.ipynb    the small webs from the paper + one exercise
│   ├── 02_hollins_web.ipynb       PageRank on a real website (300 pages)
│   └── hollins.dat                the website data
├── pca-clustering/
│   ├── pca_kmeans_survey.ipynb    the full PCA + k-Means homework
│   ├── responses_hw.csv           the survey answers
│   └── columns_hw.csv             the full text of each survey question
└── requirements.txt
```

---

## How to run

```
pip install -r requirements.txt
jupyter notebook
```

Then open a notebook and run it from top to bottom. Each notebook reads its data from its own folder.

---

## License

The code is under the [MIT License](LICENSE). The data files are not mine. See the "Data" part of each project README for where they come from.
