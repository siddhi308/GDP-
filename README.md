
# 🌍 GDP Analysis Project

This project analyzes and visualizes GDP (Gross Domestic Product) growth trends of multiple countries from 1960 to 2016 using Python. It provides tools to compare GDP growth, calculate year-on-year changes, and generate interactive graphs for both individual countries and groups.

---

## 📁 Project Structure

```
GDP-Analysis/
│
├── gdp.csv                         # Dataset with GDP values
├── gdp_analysis.ipynb             # Main analysis notebook
├── GDP All/                       # Auto-generated GDP plots for each country
├── GDP wrt world/                # Plots scaled with a fixed range for comparison
├── world_gdp_analysis.html       # World GDP interactive plot
├── GDP_analysis_of_india_china.html  # India vs China GDP comparison
├── comparison.html               # Multi-country comparison plot
└── README.md                     # Project overview and documentation
```

---

## 🛠️ Tools & Libraries Used

- **Pandas** – Data loading and manipulation
- **Plotly Express & Offline** – Interactive graph creation and HTML export
- **OS** – Directory management

---

## 🔄 Workflow Overview

### 1️⃣ **Data Loading & Cleaning**
- Loads `gdp.csv` using `pandas`
- Handles duplicates, nulls, and inspects structure

```python
df = pd.read_csv("gdp.csv")
df.isnull().sum()
df["Country Name"].describe()
df["Year"].describe()
```

---

### 2️⃣ **GDP Growth (Single Country Example)**
- Extract GDP data for a specific country like India
- Plot GDP over years
- Calculate year-on-year GDP growth %

```python
gdp_growth = [0]
for i in range(1, len(data)):
    prev = data[i-1][3]
    current = data[i][3]
    gdp_growth.append(round(((current-prev)/prev)*100, 2))
```

---

### 3️⃣ **Automate GDP Growth for All Countries**
- Iterates through all unique countries and calculates GDP growth
- Adds new `GDP_GROWTH` column

```python
final_data = []
for country_name in df["Country Name"].unique():
    ...
    df_all = df_all.assign(GDP_GROWTH=gdp_growth_all)
    final_data.append(df_all)

df = pd.concat(final_data, axis=0)
```

---

### 4️⃣ **World GDP Analysis**
- Filters world GDP and generates a line plot

```python
fig = pe.line(df_world, x='Year', y='Value', title="World GDP Analysis")
pyo.plot(fig, filename="world_gdp_analysis.html")
```

---

### 5️⃣ **Country-Wise Graph Generation**
- Generates and saves HTML plots for each country
- Two formats: normal and fixed range (e.g., 0–8 Trillion USD)

```python
for country_name in df["Country Name"].unique():
    ...
    pyo.plot(fig, filename=f"GDP wrt world/{country_name}.html", auto_open=False)
```

---

### 6️⃣ **Compare GDP of Two Countries**
- India vs China example

```python
df_IC = pd.concat([df[df["Country Name"]=="India"],
                   df[df["Country Name"]=="China"]])
fig = pe.line(df_IC, x='Year', y='Value', color='Country Name')
pyo.plot(fig, filename="GDP_analysis_of_india_china.html")
```

---

### 7️⃣ **Compare Multiple Countries**
- Custom function to dynamically compare any list of countries by code

```python
def compare_gdp(lst, is_open):
    ...
    pyo.plot(fig, filename='comparison.html', auto_open=is_open)
```

---

### 8️⃣ **Outlier Removal (Missing Year Data)**
- Filters countries with exactly 57 years of data (1960–2016)

```python
dfs = []
for country_name in df["Country Name"].unique():
    if len(df_pr) == 57:
        dfs.append(df_pr)
df_pr = pd.concat(dfs, axis=0)
```

---

## 📈 Output Examples

- 📊 `GDP_analysis_of_india_china.html` – India vs China
- 📊 `GDP wrt world/USA.html` – USA GDP scaled
- 📊 `comparison.html` – GDP trends of selected countries

---

## 📌 Conclusion

This project offers a complete pipeline for GDP trend analysis, from data preprocessing to interactive visualization. It is modular, extensible, and presentation-ready thanks to Plotly’s offline HTML output.
