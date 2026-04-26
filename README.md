# 🌍 United Nation Global Terrorism Analysis Using ML (Unsupervised Learning)

## Machine Learning Workflow

<img width="1440" height="1960" alt="image" src="https://github.com/user-attachments/assets/897ca330-295e-4efb-94ef-eb85a602f98e" />


---

## 📌 Project Overview

This project performs an in-depth Exploratory Data Analysis (EDA) and applies unsupervised machine learning techniques to the Global Terrorism Dataset. The objective is to identify temporal evolution patterns in terrorism incidents by clustering events based on how attack characteristics — such as attack type, weapon type, and target type — change across years.

The project combines data preprocessing, exploratory data analysis, multiple clustering algorithms, evaluation metrics, and interactive Power BI dashboards to deliver data-driven insights on global terrorism trends.

**Team Member:** Kolanu Padmakanth Reddy

---

## 🎯 Business Objective

This dataset helps understand global terrorism trends. Insights can help governments and organizations improve security planning, risk assessment, and counter-terrorism strategies. Specifically, the project aims to:

- Identify temporal evolution patterns in terrorism incidents across decades
- Cluster events based on attack type, weapon type, and target type characteristics
- Detect high-risk regions and countries with concentrated terrorist activity
- Analyse the distribution of casualties and severity of incidents
- Compare multiple clustering algorithms and select the best-performing model
- Build an interactive Power BI dashboard for strategic decision-making

---

## 📂 Dataset

**File:** `Global Terrorism Data.csv`

The dataset contains detailed information about terrorist incidents worldwide, spanning from 1970 to 2019.

| Column | Description |
|--------|-------------|
| `iyear` | Year of the incident |
| `country_txt` | Country where the incident took place |
| `region_txt` | Geographical region of the attack |
| `attacktype1_txt` | Type of attack (Bombing, Armed Assault, etc.) |
| `targtype1_txt` | Type of target (Civilian, Military, Government, etc.) |
| `nkill` | Number of people killed |
| `nwound` | Number of people wounded |
| `total_casualties` | Derived feature: nkill + nwound |

**Dataset Statistics (from Power BI Dashboard):**

| Metric | Value |
|--------|-------|
| Total Incidents | 181,690 |
| Total Casualties | 936,000 |
| Total Killed | 412,000 |
| Total Wounded | 524,000 |
| Average Casualties per Attack | 5.15 |

**Data Wrangling steps performed:**
- Missing categorical values filled with `"Unknown"`; numerical nulls filled with `0`
- Created derived feature `total_casualties = nkill + nwound`
- Selected only relevant columns to simplify the dataset
- Removed duplicate records to maintain data integrity
- Ensured correct data types for analysis

---

## 🔍 Exploratory Data Analysis (EDA)

EDA was performed following the UBM framework — Univariate, Bivariate, and Multivariate analysis — to uncover patterns and trends in the dataset.

### Key Insights

**Trend Over Time**
Terrorist attacks show a steady rise over the years, with a sharp increase after 2000, peaking at **16,900 incidents in 2014**, and declining to **10,900 by 2019**. This reflects escalating global security challenges in the 2010s.

**Top Affected Countries**
Attacks are concentrated in a few nations. The top countries by number of incidents are Iraq (≈30K), Pakistan, Afghanistan, India, Colombia, Philippines, and Peru — indicating clear regional hotspots requiring focused security attention.

**Attack Type Distribution**
A small number of attack types dominate the dataset:

| Attack Type | Count | Share |
|-------------|-------|-------|
| Bombing / Explosion | 88,260 | 48.57% |
| Armed Assault | 42,670 | 23.48% |
| Assassination | 19,310 | 10.63% |
| Hostage Taking (Kidnapping) | 11,160 | 6.14% |
| Facility / Infrastructure Attack | 10,360 | 5.70% |
| Other types | — | ~5.5% |

**Regional Distribution**
The Middle East & North Africa and South Asia experience significantly higher attack volumes compared to all other regions. Sub-Saharan Africa shows rapid growth since 2010.

**Casualty Distribution**
The distribution is highly right-skewed — the majority of attacks result in low casualties, while a small number of events cause extremely high human loss. Bombing/Explosion accounts for nearly all total casualties, far ahead of any other attack type.

---

## 🤖 Machine Learning Approach

Unsupervised learning was applied to identify hidden temporal evolution patterns. All algorithms were applied to **2000 samples** with **k = 5 clusters**, using features: `iyear`, `attack` (encoded), `weapon` (encoded), `target` (encoded), standardised with `StandardScaler`.

### 1️⃣ Hierarchical Clustering (Ward Linkage)
- Builds a dendrogram using Ward linkage method
- Clusters cut at `t=5` (5 clusters, `criterion='maxclust'`)
- Provides clear visual insight via dendrogram
- Limited to 2000 samples due to high computational complexity — not scalable to full dataset

### 2️⃣ K-Means
- Standard centroid-based algorithm
- `n_clusters=5`, `random_state=42`
- Produces stable, reproducible clusters
- Slower on large datasets compared to Mini-Batch variant

### 3️⃣ Mini-Batch K-Means ⭐ Best Model
- Processes data in batches of 1000 for speed and memory efficiency
- `n_clusters=5`, `batch_size=1000`, `random_state=42`
- Produces results comparable to standard K-Means
- Scalable to the full Global Terrorism Database
- **Selected as the best model** for this analysis

### 4️⃣ Gaussian Mixture Model (GMM)
- Probabilistic soft-clustering approach
- `n_components=5`, `random_state=42`
- Allows overlapping cluster assignments
- More complex to interpret; shows some cluster overlap in PCA visualisation

---

## 📊 Model Evaluation Metrics

All four algorithms evaluated using three standard unsupervised learning metrics:

| Algorithm | Silhouette Score ↑ | Davies-Bouldin ↓ | Calinski-Harabasz ↑ |
|-----------|:-----------------:|:----------------:|:-------------------:|
| Hierarchical | 0.4674 | 0.8337 | 1572.34 |
| K-Means | 0.4443 | 0.9108 | 1338.38 |
| **Mini-Batch K-Means** | **0.4791** | **0.8066** | **1658.25** |
| GMM | 0.3784 | 3.0447 | 778.68 |

**Metric guide:**
- **Silhouette Score** — closer to 1.0 is better; measures cluster cohesion vs separation
- **Davies-Bouldin Index** — lower is better; measures average similarity between clusters
- **Calinski-Harabasz Score** — higher is better; ratio of between-cluster to within-cluster dispersion

Mini-Batch K-Means achieves the **best score across all three metrics**, confirming it as the optimal choice.

---

## 🏆 Best Model Selection

**Mini-Batch K-Means** was selected as the best-performing algorithm because:

- Achieves the highest Silhouette Score (0.4791) — best cluster cohesion and separation
- Achieves the lowest Davies-Bouldin Index (0.8066) — most distinct clusters
- Achieves the highest Calinski-Harabasz Score (1658.25) — strongest cluster dispersion ratio
- Efficiently handles large datasets by processing data in small batches (batch size = 1000)
- Significantly reduces computational time and memory usage compared to Hierarchical Clustering
- Scalable and suitable for real-world large-scale datasets like the Global Terrorism Database

---

## 📊 Power BI Dashboard

The Power BI dashboard is divided into four interactive pages: **Home**, **Overview**, **Risk Analysis**, and **Advanced Insights**.

<img width="251" height="201" alt="image" src="https://github.com/user-attachments/assets/2bc34be3-5274-4a7b-ba48-d7adcf5b1603" />

---

### 📍 Page 1 — Home (Overview Dashboard)

<img width="1292" height="730" alt="Screenshot 2026-04-26 174044" src="https://github.com/user-attachments/assets/78dec70a-4f0b-498f-b013-3fb4ab60056e" />


**KPI Cards:**
- Total Incidents: **181.69K**
- Total Casualties: **936K**
- Total Killed: **412K**
- Total Wounded: **524K**
- Average Casualties per Attack: **5.15**

**Visualizations:**
- **Trend of Terrorist Attacks Over Years** — dual-axis combo chart showing incident count (bars) and total casualties (line) from 1970–2019, clearly showing the 2014 peak of 16,900 attacks
- **Major Terrorist Attack Types** — donut chart showing Bombing/Explosion dominates at 48.57%, followed by Armed Assault at 23.48%

---

### 📍 Page 2 — Overview Dashboard

<img width="1297" height="736" alt="Screenshot 2026-04-26 174116" src="https://github.com/user-attachments/assets/528cff5d-d2c2-4f6d-a4b4-92b10ef9ebc3" />


**Visualizations:**
- **Terrorism Trend Over Years** — annotated line chart with exact values per decade (0.7K in 1970 → 16.9K peak in 2014 → 10.9K in 2019)
- **Global Terrorism Hotspots** — world map showing geographical distribution of attacks across all regions
- **Distribution of Attack Types** — pie chart showing Bombing/Explosion at 48.57%, Armed Assault 23.48%, Assassination 10.63%, and remaining types

---

### 📍 Page 3 — Risk Analysis Dashboard

<img width="1288" height="725" alt="Screenshot 2026-04-26 174139" src="https://github.com/user-attachments/assets/d6167a56-d1bf-4199-81b8-870fd2f47919" />


**Visualizations:**
- **Top 10 Countries by Attacks** — horizontal bar chart: Iraq leads at ~30K incidents, followed by Pakistan, Afghanistan, India, Colombia, Philippines, Peru
- **Attack Type by Region** — stacked bar chart showing the proportion of each attack type (Armed Assault, Assassination, Bombing, etc.) across Middle East & North Africa, South Asia, South America, Sub-Saharan Africa, Western Europe, and Southeast Asia
- **Casualties Breakdown** — 100% stacked bar chart comparing total casualties contribution per attack type; Bombing/Explosion dominates
- **High Risk Countries by Attacks and Casualties** — scatter plot mapping all countries by incident count vs total casualties, identifying critical high-risk zones

---

### 📍 Page 4 — Advanced Insights Dashboard

<img width="1291" height="722" alt="Screenshot 2026-04-26 174215" src="https://github.com/user-attachments/assets/13f70642-67a1-4780-b44b-b17aa319d3e7" />


**Visualizations:**
- **Region-wise Terrorism Trend Over Time** — multi-line area chart showing how each region's attack count evolved from 1970–2019; Middle East & North Africa and South Asia show sharpest growth after 2010, peaking at ~15K–20K incidents

**Key Strategic Insights panel:**
- Most terrorist attacks result in low casualties; only a few lead to extremely high human loss
- High-casualty attacks are rare but create the greatest security, economic, and social impact
- Certain regions show continuous growth in terrorist incidents over multiple years, indicating long-term instability
- Bombing and Explosion remain the most frequent and most destructive attack methods
- Some countries experience both high attack frequency and high casualty rates, making them critical high-risk zones
- Civilian and public infrastructure targets have increased significantly compared to government-focused attacks

---

## 📈 Machine Learning Insights

The five Mini-Batch K-Means clusters represent distinct temporal phases in the evolution of terrorism:

| Cluster | Interpretation |
|---------|---------------|
| **Cluster 1** | Early-stage terrorism patterns with limited attack diversity |
| **Cluster 2** | Increasing weapon diversity indicating evolving strategies |
| **Cluster 3** | Shift towards civilian and public infrastructure targets |
| **Cluster 4** | More complex and organised attack strategies |
| **Cluster 5** | Modern and rapidly evolving terrorism patterns |

**Per-cluster analysis performed:** cluster distribution, average incident year, average attack type, average weapon type, average target type.

PCA (2 components) was applied to visualise all four clustering results in 2D scatter plots, allowing direct visual comparison of cluster separation quality across algorithms.

---

## 🛠️ Technologies Used

| Category | Tools |
|----------|-------|
| **Language** | Python |
| **Data handling** | Pandas, NumPy |
| **ML algorithms** | Scikit-learn — KMeans, MiniBatchKMeans, GaussianMixture, PCA, LabelEncoder, StandardScaler |
| **Hierarchical clustering** | SciPy — linkage, dendrogram, fcluster |
| **Visualization** | Matplotlib, Seaborn |
| **Dashboard** | Microsoft Power BI |

---

## 🚀 Key Achievement

Developed an end-to-end unsupervised machine learning pipeline on the Global Terrorism Database — performing full EDA across 181,690 incidents spanning 50 years, implementing and benchmarking four clustering algorithms, and selecting Mini-Batch K-Means as the best model with a Silhouette Score of 0.479 and Calinski-Harabasz Score of 1658. Delivered findings through an interactive four-page Power BI dashboard providing actionable strategic insights for security planning and risk assessment.

---

## ✅ Conclusion

The exploratory data analysis and unsupervised machine learning applied to the Global Terrorism Dataset reveal important patterns in terrorist activities across years, regions, countries, and attack types.

Terrorist incidents show a general increase over time, peaking in 2014, with the Middle East & North Africa and South Asia being the most severely affected regions. Bombing and Explosion is the dominant attack method, accounting for nearly half of all incidents and the majority of casualties. Casualty distribution is highly skewed — most attacks cause limited harm, but rare high-impact events create enormous damage.

Mini-Batch K-Means emerged as the most effective clustering algorithm, achieving the best scores across all three evaluation metrics while remaining scalable to the full dataset. The five resulting clusters clearly identify distinct temporal phases in terrorism evolution — from early limited-diversity attacks to modern rapidly-evolving patterns.

These insights can support governments and organisations in improving security planning, risk assessment, investment decisions, and counter-terrorism strategy development.
