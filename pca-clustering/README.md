# PCA and k-Means on a Survey of Young People

This project takes a long survey (93 questions per person) and turns it into **5 simple profiles**. Then it finds **groups of people** with similar answers.

---

## The idea in short

1. We have a survey of young people: music, movies, hobbies, fears, health, personality and spending. Most answers are a score from 1 to 5.
2. Each student gets a personal part of the data. Mine has **506 people and 93 questions**.
3. **PCA** squeezes the 93 questions into **5 main directions**. We look at each one and give it a name.
4. **k-Means** finds groups of people in this 5-direction space. The best number of groups is **3**.
5. We check the groups against things the model **never saw**: gender, education and home town.

The homework imagines a company that wants to describe its customers with a few easy profiles. So a small, readable result matters more than keeping all the information.

**Main finding:** the 3 groups line up strongly with **gender**, even though gender was not used. Group 1 is 82% women. Group 2 is 85% men. Education and home town do not change much between groups.

---

## The steps

1. **Encode:** 3 questions have text answers with a natural order (Alcohol, Punctuality, Internet usage). We turn them into numbers that keep the order, for example never = 1, social drinker = 2, drink a lot = 3.
2. **Scale:** **StandardScaler**, so every question has mean 0 and spread 1. (The scaler was picked for each student by the homework code.)
3. **PCA:** reaching 33% of the total variance needs 9 directions. The homework rule is "use at most 5", so we keep **5**. Together they keep **25.2%** of the variance.
4. **Name the directions:** for each one, we look at the questions with the biggest weights (above 0.18 or below −0.18).
5. **k-Means:** we try 3 to 10 groups and pick the one with the best **silhouette score** (how well separated the groups are).
6. **Check the groups:** against the demographic answers (outside check) and with the silhouette of each group (inside check).

---

## Main results

**1. The 5 directions (principal components)**

| PC | Variance | Name | High score means | Low score means |
|---|---|---|---|---|
| 1 | 6.9% | Romantic & Artistic Sensitivity | Dancing, Shopping, Romantic movies, Fantasy, Musicals, Writing notes, Life struggles | (no strong questions) |
| 2 | 6.3% | Social & Consumer Lifestyle | Branded clothing; spending on looks, gadgets and going out; Fun with friends, Number of friends, Socializing, Energy, Hip hop | (no strong questions) |
| 3 | 5.1% | Intellectual & Cultural vs Pop Culture | History, Science and technology, Country, Swing/Jazz, Alternative, Opera | Celebrities, Shopping |
| 4 | 3.5% | Melancholic & Reflective vs Organized & Happy | Reading, Art exhibitions, Alternative, Loss of interest, Fake, Changing the past | Prioritising workload, Happiness in life, Energy levels |
| 5 | 3.4% | Tech-Savvy & Practical | Spending on gadgets, Internet, Science and technology, Action movies, Thinking ahead, Borrowed stuff, Health | (no strong questions) |

The full text of each question is in `columns_hw.csv`.

**2. The groups (k-Means)**

The best number of groups is **k = 3** (silhouette 0.171).

| Group | Name | People | Women | Silhouette | What makes it different |
|---|---|---|---|---|---|
| 1 | Social Romantics | 212 | 82% | 0.192 | High on PC1 and PC2 |
| 2 | Practical Socializers | 158 | 15% | 0.174 | Very low on PC1, high on PC2 |
| 3 | Independent Intellectuals | 136 | 71% | 0.136 | Very low on PC2, close to average on the rest |
| All | | 506 | 58% | 0.171 | |

**3. The outside check**

- **Gender:** very different between groups (see the table). The groups found a real pattern without being told about gender.
- **Education:** similar in every group. Most people in each group finished secondary school.
- **Home town (city or village):** similar in every group. Most people in each group grew up in a city.

---

## The files

| File | What it does |
|---|---|
| `pca_kmeans_survey.ipynb` | The full homework: encoding, scaling, PCA, naming the PCs, k-Means and both checks |
| `responses_hw.csv` | The survey answers (674 people, 150 columns) |
| `columns_hw.csv` | The full question for each column, and its short name |

The notebook is uploaded **as it was run**, with its outputs and plots.

---

## How to run

- Python 3 with **NumPy, pandas, matplotlib and scikit-learn**, plus Jupyter.
- The notebook reads both CSV files from the same folder.
- Run it from top to bottom.

---

## Data

**What it is:** the **Young People Survey**, collected in 2013. Students of a statistics class at a university in Bratislava (Slovakia) asked their friends to fill it in. The questions cover 8 topics: music, movies, hobbies, phobias, health, personality, spending and demographics.

**The original:** 1,010 people and 150 columns, on [Kaggle](https://www.kaggle.com/datasets/miroslavsabo/young-people-survey). Please check the license there.

**The version used here:** the course gave a **cleaned version** with **674 people** and no missing answers. These are the two CSV files in this folder.

**My personal part:** the first code cell (given by the teacher, not to be changed) uses my student ID as a random seed. It keeps **3/4 of the people (506)** and **2/3 of the questions (93)**. The 10 demographic questions are kept apart and used only for the outside check.

---

## Good to know

- **The data part depends on the student ID.** With a different ID you get different people, different questions and different results.
- **5 PCs keep only about 25% of the variance.** The answers are spread over many directions, with no single strong pattern. The "at most 5" rule comes from the homework.
- **A silhouette of 0.17 is low.** The groups overlap a lot. Read them as tendencies, not sharp boxes.
- **The results are repeatable.** k-Means uses a fixed random seed. Running the notebook again gives the same numbers.
- The notebook plots only the **Gender** label. The comments about Education and Home Town Type come from an earlier run of the same notebook.
- The numbers in this README are the ones saved inside the notebook.
