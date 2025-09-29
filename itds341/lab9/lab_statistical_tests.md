
# Lab: Statistical Test

**General Instructions (Read Carefully):**

- For each task, store your answer in the variable name specified in the task (e.g., `df`, `male_rating_df`, etc.).
- Do **not** change the name of the variables. Your code will be autograded based on these names.
- Unless a task explicitly asks you to modify a DataFrame, do **not** mutate or overwrite it. If you need to change a DataFrame for a later task, save the original as a new variable before making changes.
- Each task should be completed independently, unless otherwise stated. Do **not** rely on the output of previous tasks unless the instructions say so.
- You may print your variable to check your answer with the expected output provided, but it is not required for grading.
- Use only the required columns or rows as specified in each task.
- You may use any valid pandas/numpy/scipy method unless a specific method is requested.
- Your code should be reproducible and not rely on previous outputs unless stated.

**Tips for Autograder-Friendly Code:**

- Always use the exact variable names specified.
- If you need to mutate a DataFrame, save a copy of the original DataFrame in a new variable before making changes, so earlier tasks can still be checked.
- If you are unsure, ask yourself: "If the autograder checks my variable, will it match the expected output exactly?"

---

## Hypothesis Testing

The goal of hypothesis testing is to answer the question, “Given a sample and an apparent effect, what is the probability of seeing such an effect by chance?”

1. The first step is to quantify the size of the apparent effect by choosing a test statistic (t-test, ANOVA, etc). 
2. The next step is to define a null hypothesis, which is a model of the system based on the assumption that the apparent effect is not real.
3. Then compute the p-value, which is the probability of the null hypothesis being true.
4. Finally interpret the result of the p-value, if the value is low, the effect is said to be statistically significant, which means that the null hypothesis may not be accurate.

## Import Libraries

```python
import pandas as pd
import numpy as np
import scipy.stats
import matplotlib.pyplot as plt
import seaborn as sns
```

---

## Task 1: Load the Teaching Rating Dataset

**T1**: Read the teaching rating dataset from `teachingratings.csv` into a DataFrame named `df`.

---

## Task 2: Does Gender Affect Teaching Evaluation? (Independent Samples t-Test)

In this section, we will test whether `gender` affects teaching evaluation ratings (`eval`).

**Assumptions for the independent samples t-test**:

- Observations are independent within and across groups.
- Each group is approximately normally distributed.
- Variances are equal across groups (homoscedasticity).

### T2-1: State the Hypotheses

Please provide your answer in the text cell below. What are the Null Hypothesis (H0) and the Alternative Hypothesis (H1)?

- Null Hypothesis (**H0**): YOUR ANSWER
- Alternative Hypothesis (**H1**): YOUR ANSWER

### T2-2: Split the Dataset by Gender

Split the dataset into the teachers' ratings from males and females. Store the results in `male_rating_df` and `female_rating_df`.

```python
print(male_rating_df)
print(female_rating_df)

# Expected output
# 4      4.5
# 5      4.0
# 6      2.1
# 7      3.7
# 8      3.2
#       ... 
# 450    3.2
# 451    4.3
# 458    3.5
# 460    4.0
# 461    4.3
# Name: eval, Length: 268, dtype: float64
# 0      4.3
# 1      3.7
# 2      3.6
# 3      4.4
# 9      4.3
#       ... 
# 455    4.1
# 456    3.3
# 457    2.3
# 459    3.5
# 462    3.0
# Name: eval, Length: 195, dtype: float64
```

### T2-3: Compute Mean and Standard Deviation

Compute the mean and the standard deviation of each group. Print the results as shown below.

```python
# Expected outputs
# male 4.06902985163589 0.5566518074142298
# female 3.901025635156876 0.5388025823261163
```

### T2-4: Test for Normality

#### T2-4-1: Visual Inspection

Please use `sns.histplot` to visually inspect whether the `'eval'` column is normally distributed.

Please provide your answer in the text cell below whether the data are normally distributed.

#### T2-4-2: Shapiro-Wilk Test

Please use `scipy.stats.shapiro` to test for normality.

Please provide your answer in the text cell below whether the data are normally distributed.

### T2-5: Test for Equal Variances

Plase use `scipy.stats.levene` to test if the variances of teaching evaluation rates are equal. Specify `center` based on your conclusion in T2-4-1.

**Hint**:

- `center='mean'` is used when the data are normally distributed.
- `center='median'` is used when the data are not normally distributed.

Please provide your answer below whether two samples have the same variances.

### T2-6: Perform the Independent Samples t-Test

Please use `scipy.stats.ttest_ind` to perform the independent samples t-test. Specify `equal_var` based on the Levene's test results in T2-5.

Please provide your answer below whether the gender affects teaching evaluation rating based on the t-test result.

### T2-7: Perform the Mann-Whitney U-Test

Please use `scipy.stats.mannwhitneyu` to test whether gender affects teaching evaluation rates.

Please provide your answer below whether the gender affects teaching evaluation rating based on the Mann-Whitney U-Test result.

---

## Task 3: Does Teaching Evaluation Differ by Age? (One-way ANOVA)

In this section, you will use one-way ANOVA to determine if teaching evaluation scores differ by age group.

**Assumptions for one-way ANOVA**:

- Observations are independent.
- Each group is normally distributed.
- Variances are equal across groups.

In this task, we will assume that the data in each age group are normally distributed. Thus the only test left before we can run the one-way ANOVA is the test for the equality of variances.

### T3-1: Split the Dataset by Age Group

Split the dataset into three age groups: young, mid, and old. Store the results in `young_rating_df`, `mid_rating_df`, and `old_rating_df`.

```python
young_rating_df = df.loc[(df['age'] <= 40), 'eval']
mid_rating_df = df.loc[(df['age'] > 40) & (df['age'] < 57), 'eval']
old_rating_df = df.loc[(df['age'] >= 57), 'eval']
```

### T3-2: Compute Mean and Standard Deviation for Each Group

Compute the mean and the standard deviation of each group. Print the results as shown below.

```python
# Expected outputs

# young 4.002654871054455 0.5057632493708224
# mid 4.030701748111792 0.5379234358312474
# old 3.9336065581587496 0.6242497286958886
```

### T3-3: Test for Equal Variances

Please use `scipy.stats.levene` to test for equality of variances among the three age groups. Use `center='mean'` as we assume normality.

Hint: As `scipy.stats.levene` support more than two sets of samples, so you can run the test on three groups in one go.

Please provide your answer below whether the variances are the same or different.

### T3-4: Perform One-way ANOVA

Regardless of whether the assumption on the same variances hold from the task T3-3, please Use `scipy.stats.f_oneway` to perform one-way ANOVA.

Please provide your answer below whether the teaching evaluation rating differs by age group.
