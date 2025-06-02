# Student Performance Analysis

## **1. Introduction**

The purpose of this analysis is to investigate the factors associated with students’ academic performance, as measured by their exam scores. Specifically, the analysis examines whether exam performance differs based on categorical lifestyle and demographic factors such as part-time job status, diet quality, exercise frequency, and internet quality. Additionally, it explores potential linear relationships between exam scores and continuous variables including age, study hours, sleep duration, and media consumption.

To ensure valid statistical conclusions, assumption checks were conducted for each hypothesis test, and both parametric and non-parametric methods were employed as appropriate. Where overall group differences were detected, post hoc tests were used to identify which specific group comparisons were significant.

---

## 2. Dataset Description

This dataset comprises responses from 1,000 students aimed at exploring the relationship between various lifestyle, demographic, and psychological factors and academic performance, measured through **exam_score**. The dataset includes 15 variables (excluding the target variable), spanning categorical, binary, and continuous types. Below is a detailed description of each variable:

---

### **Variable Descriptions**

1. **age** *(Continuous)*
    - **Description:** Age of the student in years.
    - **Stats:** Mean = 20.50, Std = 2.31, Min = 17, Max = 24
2. **gender** *(Binary)*
    - **Description:** Gender of the student (0 = Male, 1 = Female).
    - **Stats:** 958 non-missing values, roughly balanced (Mean = 0.50)
3. **study_hours_per_day** *(Continuous)*
    - **Description:** Average number of hours spent studying each day.
    - **Stats:** Mean = 3.55 hrs, Std = 1.47, Min = 0, Max = 8.3
4. **social_media_hours** *(Continuous)*
    - **Description:** Average number of hours spent on social media per day.
    - **Stats:** Mean = 2.51 hrs, Std = 1.17, Min = 0, Max = 7.2
5. **netflix_hours** *(Continuous)*
    - **Description:** Average number of hours spent watching Netflix per day.
    - **Stats:** Mean = 1.82 hrs, Std = 1.08, Min = 0, Max = 5.4
6. **part_time_job** *(Binary)*
    - **Description:** Whether the student has a part-time job (0 = No, 1 = Yes).
    - **Stats:** 21.5% have part-time jobs (Mean = 0.215)
7. **attendance_percentage** *(Continuous)*
    - **Description:** Percentage of classes attended.
    - **Stats:** Mean = 84.13%, Std = 9.40, Min = 56%, Max = 100%
8. **sleep_hours** *(Continuous)*
    - **Description:** Average number of sleep hours per night.
    - **Stats:** Mean = 6.47 hrs, Std = 1.23, Min = 3.2, Max = 10
9. **diet_quality** *(Ordinal Categorical: 0 = Poor, 1 = Average, 2 = Good)*
    - **Description:** Self-rated quality of diet.
    - **Stats:** Mean = 1.19, indicating diet quality tends toward “Average”
10. **exercise_frequency** *(Ordinal Categorical: 0–6)*
    - **Description:** Number of days per week the student exercises.
    - **Stats:** Mean = 3.04 days, Std = 2.03
11. **parental_education_level** *(Ordinal Categorical: 0 = None, 1 = High School, 2 = College)*
    - **Description:** Highest education level attained by the student’s parents.
    - **Stats:** Available for 909 students. Mean = 0.75
12. **internet_quality** *(Ordinal Categorical: 0 = Poor, 1 = Moderate, 2 = Good)*
    - **Description:** Student’s assessment of their home internet quality.
    - **Stats:** Mean = 1.29, skewed toward “Moderate to Good”
13. **mental_health_rating** *(Ordinal Categorical: 1–10)*
    - **Description:** Self-rated mental health on a 1 (low) to 10 (high) scale.
    - **Stats:** Mean = 5.44, Std = 2.85, Min = 1, Max = 10
14. **extracurricular_participation** *(Binary)*
    - **Description:** Whether the student participates in extracurricular activities (0 = No, 1 = Yes).
    - **Stats:** 31.8% participate (Mean = 0.318)
15. **exam_score** *(Continuous)*
    - **Description:** Final exam score, used as the outcome variable.
    - **Stats:** Mean = 69.60, Std = 16.89, Min = 18.4, Max = 100

---

### **Missing Data Note**

- **parental_education_level** has 91 missing entries.

These missing values were handled appropriately during the analysis phase to maintain the integrity of statistical results.

---

## **3. Data Preparation and Cleaning**

Before conducting the analysis, several data preprocessing steps were carried out to ensure data quality and consistency:

- **Handling Missing Values**: A total of 91 records with missing values in the *Parental Education Level* field were removed to preserve the validity of group comparisons.
- **Categorical Encoding**: Categorical variables such as *Gender* and *Parental Education Level* were converted into numerical formats using label encoding or dummy variables to facilitate statistical testing and correlation analysis.
- **Irrelevant Column Removal**: Non-informative columns like *Student ID* were dropped to streamline the dataset and avoid potential biases in analysis.
- **Data Type Verification**: All variables were reviewed to ensure appropriate data types (e.g., numeric vs. categorical), and any inconsistencies or formatting issues were corrected.

These steps helped create a clean, analysis-ready dataset suitable for hypothesis testing and statistical modeling.

---

## **4. Exploratory Data Analysis (EDA)**

### **General Distributions**

**Histograms**

![png](https://github.com/jflores31297/portfolio/blob/main/assets/StudentPerformanceAnalysis/01.png?raw=true)

**Count plots**

![Screenshot 2025-05-30 at 12.17.11 AM.png](Final%20Report%20202509a087318042b4e7d1057e99aed1/Screenshot_2025-05-30_at_12.17.11_AM.png)

### **Relationship with Exam Score**

**Scatter plots:**

![Screenshot 2025-05-30 at 12.18.18 AM.png](Final%20Report%20202509a087318042b4e7d1057e99aed1/Screenshot_2025-05-30_at_12.18.18_AM.png)

**Boxplots:**

![Screenshot 2025-06-01 at 6.51.21 PM.png](Final%20Report%20202509a087318042b4e7d1057e99aed1/Screenshot_2025-06-01_at_6.51.21_PM.png)

**Correlation matrix heatmap:**

![CorrelationMatrix.png](Final%20Report%20202509a087318042b4e7d1057e99aed1/CorrelationMatrix.png)

---

## **5. Statistical Analysis**

### **Correlation Analyses**

To examine continuous predictors of exam performance, Pearson correlation coefficients were calculated between exam scores and several continuous variables. The strength and direction of each relationship, as well as statistical significance, are described below.

**Mental Health Rating**

A **moderate positive correlation** was observed between students’ mental health ratings and exam performance (**r = 0.318, p < 0.001**). This indicates that **better self-reported mental health is associated with higher exam scores**, suggesting that psychological well-being may play a supportive role in academic success.

**Age**

There was **no significant relationship** between age and exam scores (**r = -0.013, p = 0.699**), suggesting that **age does not predict performance** in this student sample.

**Study Hours**

The strongest observed correlation was between **study hours and exam scores** (**r = 0.823, p < 0.001**). This represents a **very strong positive association**, confirming that **increased study time is strongly linked to better academic performance**. This is consistent with well-established educational findings and highlights the importance of sustained study habits.

**Social Media Hours**

A **small but significant negative correlation** was found between social media usage and exam performance (**r = -0.172, p < 0.001**). This suggests that **more time spent on social media is associated with lower exam scores**, possibly due to time displacement or reduced concentration.

**Netflix Hours**

A similar pattern was observed for time spent watching Netflix, with a **negative correlation** to exam performance (**r = -0.167, p < 0.001**). Like social media, this suggests that excessive screen time for entertainment may be **detrimental to academic outcomes**.

**Attendance**

There was a **small but statistically significant positive correlation** between attendance and exam scores (**r = 0.096, p = 0.004**). This suggests that **students who attend more classes tend to perform slightly better**, though the effect size is modest.

**Sleep Hours**

Lastly, a **small but significant positive relationship** was found between sleep duration and exam scores (**r = 0.122, p < 0.001**). This implies that **getting more sleep is associated with improved academic performance**, reinforcing the importance of sleep hygiene in student success.

---

**T-Tests**

**Part-Time Job Status**

To determine whether students’ exam scores differ based on part-time job status, both an independent samples t-test (Welch’s correction applied due to violation of normality) and a non-parametric Mann-Whitney U-test were conducted. The t-test yielded a **t-statistic of 0.515** with a **p-value of 0.607**, and the Mann-Whitney U-test yielded a **U-statistic of 71,900.500** with a **p-value of 0.534**. Both p-values are well above the conventional significance level of 0.05, indicating **no statistically significant difference** in exam scores between students with and without a part-time job. These results suggest that part-time employment is **not associated** with a meaningful difference in academic performance in this sample.

**Gender**

An independent samples t-test and Mann-Whitney U-test were also conducted to evaluate whether exam scores differ by gender. The t-test produced a **t-statistic of -0.874** with a **p-value of 0.382**, while the Mann-Whitney U-test resulted in a **U-statistic of 91,796.000** and a **p-value of 0.352**. As with part-time job status, these findings are not statistically significant. Therefore, **no evidence was found to suggest a difference in exam performance based on gender** in this dataset.

**Extracurricular Participation**

Lastly, the relationship between extracurricular involvement and exam scores was examined. The t-test produced a **t-statistic of -0.103** with a **p-value of 0.918**, and the Mann-Whitney U-test yielded a **U-statistic of 88,898.500** with a **p-value of 0.851**. Both tests strongly indicate a **lack of significant association** between participation in extracurricular activities and exam performance.

---

### **Group Comparisons Using ANOVA and Kruskal-Wallis Tests**

**Diet Quality**

To assess whether exam performance varies based on students’ diet quality, both an ANOVA and Kruskal-Wallis test were conducted. The **ANOVA returned F = 0.792 with p = 0.453**, and the **Kruskal-Wallis test returned H = 1.623 with p = 0.444**. Both results indicate **no statistically significant differences** in exam scores across diet quality categories. Thus, diet quality does not appear to be associated with exam performance in this sample.

**Exercise Frequency**

In contrast, results for exercise frequency showed a significant association with exam performance. The **ANOVA yielded F = 5.546 with p < 0.001**, and the **Kruskal-Wallis test returned H = 28.839 with p < 0.001**, both indicating **statistically significant differences** in exam scores across exercise frequency levels.

To further explore which groups differed, a **Dunn’s post hoc test** with Bonferroni correction was performed. Key findings include:

Students who **exercised daily (6 days/week)** scored significantly higher than those who:

- Did **not exercise at all (0 days/week)** *(p = 0.004)*
- Exercised **once per week** *(p < 0.001)*
- Exercised **3 days/week** *(p = 0.049)*

The comparison between **once per week and 5 days/week** was also significant *(p = 0.018)*.

These results suggest that **more frequent exercise is positively associated with exam performance**, with the most notable gains occurring at the highest frequency of physical activity.

**Parental Education Level**

Group comparisons based on parental education level revealed **no significant differences** in exam scores. The **ANOVA yielded F = 0.942, p = 0.390**, and the **Kruskal-Wallis test returned H = 2.991, p = 0.224**. These results imply that the educational attainment of students’ parents did not have a measurable impact on academic performance in this context.

**Internet Quality**

Finally, the impact of internet quality on exam performance was examined. Although some evidence of non-normality was observed in earlier diagnostics, neither the **ANOVA (F = 1.518, p = 0.220)** nor the **Kruskal-Wallis test (H = 3.060, p = 0.217)** found significant differences across internet quality levels. This suggests that, within this sample, perceived internet quality did not significantly influence exam outcomes.

---

## **6. Final Summary and Recommendations**

This study explored a range of personal, behavioral, and contextual factors to understand what influences students’ academic performance, as measured by exam scores. By applying a combination of hypothesis testing, group comparisons, and correlation analyses, several key insights emerged.

### **Key Findings**

- **Study habits matter most**: The strongest predictor of exam performance was the number of hours spent studying (**r = 0.823, p < 0.001**). This robust association highlights the critical role of consistent and focused study in achieving academic success.
- **Mental health and sleep contribute meaningfully**: Better self-rated mental health (**r = 0.318**) and adequate sleep duration (**r = 0.122**) were both positively associated with higher exam scores, suggesting that well-being and rest should not be overlooked as academic assets.
- **Exercise may help, especially at higher frequencies**: Students who exercised more frequently (especially 6 times per week) outperformed those with little to no exercise. While overall effect sizes were modest, this supports the idea that regular physical activity may enhance cognitive performance or mood in a way that benefits learning.
- **Digital distractions hinder performance**: Time spent on social media (**r = -0.172**) and Netflix (**r = -0.167**) showed small but significant negative relationships with exam performance. This suggests that excessive screen time may interfere with study time or focus.
- **Other demographic and contextual factors had little effect**: Variables such as part-time job status, gender, extracurricular participation, parental education, diet quality, internet quality, and age showed no significant or meaningful relationships with exam performance in this dataset.

### **Recommendations**

Based on these findings, the following evidence-based recommendations are offered for students and educators:

### **For Students:**

1. **Prioritize consistent study routines**: Regular and focused study sessions are the most powerful predictor of academic performance. Aim for daily dedicated time, and minimize multitasking.
2. **Protect your mental health**: Seek support when stressed, maintain a balanced routine, and use mental health resources offered by your institution.
3. **Establish healthy sleep habits**: Aim for at least 7–9 hours of quality sleep each night. Avoid late-night screen time and build a calming bedtime routine.
4. **Incorporate physical activity**: Engage in moderate exercise most days of the week. Even a short daily walk or workout can contribute positively to academic outcomes.
5. **Limit digital distractions**: Set boundaries around social media and streaming time—consider app timers or study apps like Forest or Pomodoro tools to stay on task.

### **For Educators and Institutions:**

1. **Promote study skills and time management workshops**: Equip students with strategies to study more effectively and manage distractions.
2. **Support student well-being**: Offer accessible mental health resources and encourage practices that promote resilience and stress management.
3. **Encourage a balanced student lifestyle**: Integrate wellness initiatives, exercise programs, and sleep hygiene education into student success programming.
4. **Monitor digital engagement**: Consider educational campaigns that raise awareness about the impact of excessive screen time on academic focus and performance.
