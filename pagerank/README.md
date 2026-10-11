# PageRank

This project ranks web pages by **how important they are**, using the same idea as Google's first search engine. It follows the paper *The $25,000,000,000 Eigenvector: The Linear Algebra Behind Google* by Kurt Bryan and Tanya Leise.

---

## The idea in short

1. The web is a graph: pages, and links between them.
2. A page is important if **important pages link to it**.
3. This becomes a linear algebra problem: find a vector `x` with `M x = x`. That is an **eigenvector** of `M` for the eigenvalue 1.
4. We find it with the **power method**: start with equal scores and multiply by `M` again and again.
5. We test the code on the small examples from the paper, then on a **real website** (Hollins University).

**Main finding:** the code gives the same scores as the paper. On the real website, the home page comes first, then the admissions pages.

The code is written **from scratch with NumPy**. No graph or PageRank library is used.

---

## The method

- **Link matrix `A`:** if page `j` has `n` links and one of them goes to page `i`, then `A[i, j] = 1/n`. So each page splits its vote equally between the pages it links to.
- **Dangling pages:** a page with no links out gives a column of zeros. We fill that column with `1/N` (N = number of pages). This means "from here, jump to any page at random".
- **Google matrix `M`:** `M = (1 − m) A + m S`, where every entry of `S` is `1/N` and **m = 0.15** (the value Google used). This makes sure there is only one answer.
- **Power method:** start with `x = (1/N, ..., 1/N)`, then repeat `x = M x` **30 times**. The final scores add up to 1.

---

## Main results

**1. The examples from the paper**

Our scores are the same as the ones printed in the paper.

| Web | Our scores | In the paper |
|---|---|---|
| Fig. 2.1 (4 pages) | 0.368, 0.142, 0.288, 0.202 | 0.368, 0.142, 0.288, 0.202 |
| Fig. 2.2 (5 pages, 2 separate parts) | 0.200, 0.200, 0.285, 0.285, 0.030 | 0.2, 0.2, 0.285, 0.285, 0.03 |

Fig. 2.2 shows why we need `M`: with only `A`, the two separate parts of this web cannot be compared. With `M`, every page gets one clear score.

**2. Exercise 11 from the paper**

We take the web of Fig. 2.1 and add a **page 5**. Page 5 links to page 3, and page 3 links back to page 5.

| Page | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|
| Score | 0.237 | 0.097 | **0.349** | 0.138 | 0.178 |

Before, page 1 was first. Now **page 3 is first**. So creating a new page that links to you can raise your rank.

**3. A real website: Hollins University**

The data has 6,012 pages and 23,875 links. We used the **first 300 pages** and the 3,691 links between them. 47 of these pages are dangling.

Top 10 pages:

| Rank | Page | Score | URL |
|---|---|---|---|
| 1 | 2 | 0.0512 | www.hollins.edu/ |
| 2 | 37 | 0.0405 | www.hollins.edu/admissions/visit/visit.htm |
| 3 | 38 | 0.0377 | www.hollins.edu/about/about_tour.htm |
| 4 | 61 | 0.0369 | www.hollins.edu/htdig/index.html |
| 5 | 52 | 0.0354 | www.hollins.edu/admissions/info-request/info-request.cfm |
| 6 | 43 | 0.0307 | www.hollins.edu/admissions/apply/apply.htm |
| 7 | 27 | 0.0267 | www.hollins.edu/admissions/admissions.htm |
| 8 | 28 | 0.0240 | www.hollins.edu/academics/academics.htm |
| 9 | 29 | 0.0183 | www.hollins.edu/grad/coedgrad.htm |
| 10 | 73 | 0.0155 | www.hollins.edu/calendar |

The notebook prints the page numbers. The URLs come from `hollins.dat`.

---

## The files

| File | What it does |
|---|---|
| `01_paper_examples.ipynb` | Builds `M` and runs the power method on Fig. 2.1, Fig. 2.2 and Exercise 11 |
| `02_hollins_web.ipynb` | Reads `hollins.dat`, builds the link matrix, fixes dangling pages, ranks the pages |
| `hollins.dat` | The Hollins University web data |

The notebooks are uploaded **as they were run**, with their outputs.

---

## How to run

- Python 3 with **NumPy** and Jupyter.
- `02_hollins_web.ipynb` reads `hollins.dat` from the same folder.

---

## Data

**What it is:** a crawl of the **Hollins University** website (hollins.edu). It is a classic test set for PageRank. It was given to us by the course.

**The file format:**
- Line 1: number of pages and number of links (`6012 23875`).
- Next 6,012 lines: page number and its URL.
- Next 23,875 lines: one link per line, as `from_page to_page`.

**The paper:** Bryan, K. and Leise, T. (2006). *The $25,000,000,000 Eigenvector: The Linear Algebra Behind Google.* SIAM Review 48(3). [Link to the paper](https://doi.org/10.1137/050623280). The authors also have a [page about it](https://www.rose-hulman.edu/~bryan/google.html).

---

## Good to know

- **Only the first 300 pages** of the Hollins data are used, not all 6,012.
- The matrix is stored as a normal (dense) matrix. That is fine for 300 pages, but not for a very large web.
- **30 steps are enough here.** Running many more steps gives the same top 10.
- The paper does not deal with dangling pages. The "fill the column with 1/N" fix is a common choice, not something from the paper.
- The numbers in this README are the ones saved inside the notebooks.
