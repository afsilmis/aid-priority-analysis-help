# HELP International Aid Distribution Analysis

_Country Classification Based on Socio-Economic and Health Indicators for Strategic Aid Allocation_

---

## Background and Objectives

#### Problem Statement
HELP International, a global humanitarian NGO focused on poverty alleviation, faces a critical decision in allocating $10 million in aid funding. With limited resources and numerous countries in need, the organization requires a data-driven approach to identify priority countries for maximum humanitarian impact.

#### Why This Analysis Matters
This analysis is crucial for evidence-based decision-making, as it ensures aid is allocated effectively to countries with the most urgent needs. The framework provides transparency and accountability, and its replicable methodology makes it a reliable tool for future funding decisions.

### Expected Impact
With the analysis results, HELP International can:
- Strategically allocate $10 million to maximize humanitarian impact
- Develop targeted intervention programs based on specific country needs
- Establish clear metrics for measuring aid effectiveness
- Build evidence-based partnerships with local organizations

---

## Problem Scope and Limitations

#### Scope Definition
This analysis focuses on categorizing 167 countries based on socio-economic and health development indicators to identify nations requiring immediate humanitarian assistance. The model outputs a binary classification system distinguishing between:
- **Cluster 0**: Countries requiring urgent humanitarian aid
- **Cluster 1**: More developed countries with better living conditions

#### Machine Learning Output
The clustering model produces:
- Country classifications (0 = Aid Priority, 1 = Developed)
- Cluster centroids representing typical characteristics of each group
- Priority ranking within the aid-requiring cluster
- Recommended funding allocation strategy

#### Limitations
- Analysis limited to available socio-economic indicators (9 variables)
- Does not account for political stability, natural disasters, or conflict situations
- Static analysis - does not capture recent rapid changes in country conditions
- Equal weighting of all indicators may not reflect real-world priorities

---

## Data and Assumptions

### Dataset Overview
The analysis utilizes a comprehensive dataset containing 167 countries with 9 key development indicators:

**Health Indicators:**
- `child_mort`: Child mortality rate (deaths per 1000 live births)
- `life_expec`: Life expectancy (years)
- `health`: Healthcare expenditure per capita (USD)

**Economic Indicators:**
- `gdpp`: GDP per capita (USD)
- `income`: Net per capita income (USD)  
- `inflation`: Annual inflation rate (%)
- `trade`: Trade balance (% of GDP)

**Demographic Indicators:**
- `total_fer`: Total fertility rate (children per woman)

#### Key Assumptions
1. The 9 selected indicators adequately represent overall country development
2. All country data is accurate and represents current conditions
3. All indicators contribute equally to development assessment
4. Countries can be meaningfully divided into two distinct groups
5. Country conditions remain stable during the analysis period

#### Feature Selection Rationale
Selected features represent the most critical aspects of human development:
- Child mortality and life expectancy: Direct measures of population health
- GDP per capita and income: Economic capacity and living standards  
- Healthcare expenditure: Government investment in population wellbeing
- Fertility rate: Demographic pressure and family planning effectiveness
- Inflation and trade: Economic stability indicators

---

## Data Analysis

#### Dataset Overview and Descriptive Statistics

The dataset comprises 167 countries with complete data across all 9 indicators, with no missing values or duplicates detected. The descriptive statistics reveal significant global disparities:

| Variable | Mean | Median | Std Dev | Min | Max | Skewness |
| --- | --- | --- | --- | --- | --- | --- |
| Child Mortality | 38.27 | 19.30 | 40.33 | 2.6 | 208.0 | Highly right-skewed |
| Life Expectancy | 70.56 | 73.10 | 8.89 | 32.1 | 82.8 | Left-skewed |
| GDP per Capita | $12,964 | $4,660 | $18,329 | $231 | $105,000 | Extremely right-skewed |
| Income | $17,145 | $9,960 | $19,278 | $609 | $125,000 | Extremely right-skewed |
| Health Expenditure | $6.82 | $6.32 | $2.75 | $1.81 | $17.90 | Slight right-skew |
| Total Fertility | 2.95 | 2.41 | 1.51 | 1.15 | 7.49 | Right-skewed |
| Exports | 41.11 | 35.00 | 27.41 | 0.109 | 200.0 | Right-skewed |
| Imports | 46.89 | 43.30 | 24.21 | 0.0659 | 174.0 | Right-skewed |
| Inflation | 7.78 | 5.39 | 10.57 | -4.21 | 104.0 | Highly right-skewed |

<img width="12000" height="7200" alt="KDE plots of 9 development indicators (Child Mortality, Life Expectancy, GDP per Capita, Income, Health Expenditure, Total Fertility, Exports, Imports, Inflation) across 167 countries. Each plot includes vertical lines marking the mean (red), mode (green), and median (blue), illustrating varying degrees of skewness, with many indicators showing right-skewed distributions." src="https://github.com/user-attachments/assets/142f3784-08ed-4925-a053-faf728b4a28e" />

_Figure: Distribution and Central Tendencies of 9 Key Development Indicators Across 167 Countries_

#### Extreme Value Analysis

<img width="12000" height="9600" alt="Dot plots showing countries with extreme values in development indicators (2009), including fertility, mortality, life expectancy, income, GDP, trade, and spending. Highlights crisis countries (e.g., Haiti, Chad) and high-performing outliers (e.g., Singapore, Luxembourg)." src="https://github.com/user-attachments/assets/a2b8e78a-5470-440f-9e9f-274f5f0e943b" />

_Figure: Extreme Value Analysis of Global Development Indicators_

**Crisis Countries (Multiple Extreme Indicators):**

- **Haiti**: Worst child mortality (208), lowest life expectancy (32.1), extremely low GDP ($662)
- **Sierra Leone**: Second-highest child mortality (160), very low GDP ($399), high health expenditure needs
- **Chad**: High fertility (6.59), high child mortality (150), low life expectancy (47.5)
- **Niger**: Highest fertility rate (7.49), high child mortality (137)

**Outlier Performers:**

- **Singapore**: Extreme export economy (200% of GDP), high import dependency (174%)
- **Luxembourg**: Highest GDP per capita ($105,000), highest income ($125,000)
- **Qatar**: Oil-rich economy with high income but moderate other indicators

#### Correlation Analysis

<img width="700" alt="A heatmap visualizing the pairwise correlation matrix of development indicators. Each cell contains a correlation coefficient ranging from –1.00 (strong negative) to 1.00 (strong positive), with colors transitioning from blue (negative) to red (positive). Strong correlations are visible between income and GDP per capita, and between child mortality and fertility. Negative associations include child mortality with life expectancy and fertility with life expectancy." src="https://github.com/user-attachments/assets/edfe0cea-2885-4616-9a68-78ab5a5646ed" />

_Figure: Correlation heatmap of development indicators_

**Strong Positive Correlations (>0.8):**

- **Income & GDP per Capita**: r=0.90 (expected economic relationship)
- **Child Mortality & Fertility**: r=0.85 (demographic transition pattern)

**Strong Negative Correlations (<-0.8):**

- **Child Mortality & Life Expectancy**: r=-0.89 (health outcomes linkage)
- **Life Expectancy & Fertility**: r=-0.76 (demographic transition)

**Key Development Cluster Relationships:**

- **GDP per Capita & Life Expectancy**: r=0.60 (wealth-health connection)
- **GDP per Capita & Child Mortality**: r=-0.48 (economic development reduces child deaths)
- **Income & Health Expenditure**: r=0.61 (wealthier countries invest more in health)

#### Geographic and Regional Patterns

1. **Sub-Saharan Africa Dominance in Crisis Indicators**: 8 out of the 10 countries with the highest child mortality rates are in Sub-Saharan Africa, along with 7 out of the 10 countries with the lowest life expectancy. This pattern highlights regional, systemic issues—such as weak healthcare infrastructure, political instability, and extreme poverty—that go beyond individual country circumstances.

2. **Small Island Development States (SIDS) Pattern**: SIDS like Singapore, Malta, and Seychelles show extremely high import/export ratios due to limited domestic production capacity. With export levels reaching up to 200% of GDP in some cases, these economies are highly vulnerable to external shocks, relying heavily on global trade to sustain their economic activity.

3. **Oil-Rich Economies Anomaly**: Countries like Qatar, Brunei, and Kuwait exhibit high income levels due to resource wealth, but this doesn't always translate to better health or development outcomes. The discrepancy suggests that high GDP alone doesn’t ensure human development without broader investment in public services and social infrastructure.

#### Data Preprocessing Pipeline

**1. Data Quality Assessment**: All 167 countries had complete records with no missing values or duplicate entries. Variable distributions were checked and found to be within expected ranges, confirming consistency and reliability across the dataset.

**2. Outlier Treatment**: Highly skewed variables were log-transformed to reduce distortion. For extreme outliers, IQR-based capping was applied, effectively limiting their influence while preserving overall data structure and integrity.

**3. Feature Standardization**: All features were standardized using StandardScaler to ensure uniform scaling. This prevented any single variable from disproportionately influencing the clustering process, ensuring fair distance calculations across dimensions.

#### Dimensionality Reduction

<img width="8000" height="4000" alt="Scree plot showing cumulative explained variance across 9 principal components. A red dashed line marks the 90% variance threshold, reached at the fourth component." src="https://github.com/user-attachments/assets/63f1e61e-11a9-479b-8a0a-411eac8fa116" />

_Figure: Scree plot of cumulative explained variance from PCA. The first four components capture 90% of the total variance, as indicated by the red dashed line._

Principal Component Analysis (PCA) was used to reduce the original 9-dimensional dataset to 4 principal components. These 4 components retained 90% of the total variance, effectively capturing the essential patterns in the data while minimizing information loss. The dimensionality reduction simplified the feature space without compromising interpretability.

### Model Development and Validation

<img width="7200" height="3200" alt="Elbow plot of within-cluster sum of squares (WCSS) against number of clusters (k), showing a sharp drop at k = 2, indicating optimal clustering." src="https://github.com/user-attachments/assets/455d6ca8-1731-4b9d-a67c-d9ed55e7e6e7" />

_Figure: Elbow plot showing the within-cluster sum of squares (WCSS) for different values of k. The sharp decrease at k = 2 suggests this as the optimal number of clusters._

The elbow method was used to determine the optimal number of clusters, with k=2 emerging as the clear choice. The within-cluster sum of squares (WCSS) dropped sharply from k=1 to k=2, after which the gains diminished—indicating that two clusters best capture the structure in the data.

<img width="7200" height="4000" alt="Scatter plot showing two distinct clusters with colored data points and red X markers as centroids. Minimal overlap observed between clusters." src="https://github.com/user-attachments/assets/d0696707-443f-4396-8efc-476ba7f2b538" />

_Figure: Visualization of clustering results for k = 2. Data points are colored by cluster, with red X markers representing the centroids. The two clusters are clearly separated with minimal overlap._

Model performance showed strong, clean separation between the two clusters with minimal overlap. The cluster centroids revealed distinct profiles, reflecting meaningful differences in country development indicators. These results aligned well with expected patterns, supporting the validity of the clustering approach.

---

## 5. Results and Conclusions

### Final Model Selection
- **Algorithm**: K-Means Clustering with PCA preprocessing  
- **Optimal Clusters**: 2 (Aid Priority vs. Developed Countries)
- **Feature Set**: All 9 original indicators after standardization
- **Dimensionality**: Reduced to 4 PCA components (90% variance explained)

### Cluster Characteristics

<img width="8000" height="4000" alt="Two radar charts comparing development indicators for two clusters. Cluster 0 shows high child mortality, low GDP per capita, and low life expectancy. Cluster 1 shows low child mortality, high income, and better overall health metrics." src="https://github.com/user-attachments/assets/dda889e9-236f-42d2-9cbd-abda6fd1400b" />

_Figure: Radar charts showing feature profiles of Cluster 0 (left) and Cluster 1 (right), highlighting contrasts in development indicators._

**Cluster 0 - Countries Requiring Urgent Aid:**
- Child mortality: **74.16 per 1000** births (extremely high)
- Life expectancy: **62.28 years** (below global average)
- GDP per capita: **$2,019** (low income)
- Fertility rate: **4.33 children** per woman (high)

**Cluster 1 - Developed Countries:**
- Child mortality: **11.73 per 1000** births (very low)
- Life expectancy: **76.68 years** (above global average)  
- GDP per capita: **$21,059** (high income)
- Fertility rate: **1.93 children** per woman (low)

---

## 6. Recommendations

#### Priority Countries for Aid Allocation

**Immediate Priority - $10M Total Allocation:**

1. **Haiti** - $3M (30%)
   - Critical health crisis: 208 child deaths per 1000 births
   - Lowest life expectancy: 32.1 years
   - Emergency healthcare intervention required

2. **Sierra Leone** - $2M (20%)  
   - Extreme poverty: $399 GDP per capita
   - Healthcare infrastructure collapse
   - Focus on basic health system rebuilding

3. **Central African Republic** - $2M (20%)
   - Prolonged humanitarian crisis
   - Life expectancy: 47.5 years
   - Integrated humanitarian assistance needed

4. **Mali** - $1.5M (15%)
   - High demographic pressure: 6.55 fertility rate
   - Maternal and child health emergency
   - Population growth outpacing development

5. **Chad** - $1.5M (15%)
   - Severe demographic challenges
   - Education and nutrition crisis
   - Long-term capacity building focus

### Future Model Enhancements
1. **Real-time Data Integration**: Incorporate live updates for dynamic prioritization
2. **Conflict and Disaster Indicators**: Add political stability and natural disaster risk factors
3. **Multi-level Clustering**: Develop sub-categories within aid-priority countries
4. **Predictive Modeling**: Forecast future aid needs based on current trends
5. **Impact Assessment**: Build feedback loops to measure intervention effectiveness

---

**Author**: Az-Zukhrufu Fi Silmi Suwondo  
**Email**: afsilmis@gmail.com  
**GitHub**: [github.com/afsilmis/](https://github.com/afsilmis/)

---
*This analysis provides a data-driven foundation for humanitarian aid allocation decisions. The methodology is designed to be transparent, replicable, and scalable for future applications.*
