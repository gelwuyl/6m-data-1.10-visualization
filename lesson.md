# 🎓 Lesson 1.10: Data Visualisation & Storytelling — Instructor Guide

## Session Overview

| Item | Detail |
|------|--------|
| **Duration** | 3 hours |
| **Format** | Flipped Classroom + Guided Coding in Jupyter |
| **Prerequisites** | EDA Advanced (Lesson 1.9); Pandas GroupBy; pre-class reading on Perception/Design/Storytelling |
| **Tools** | VS Code + `pds` conda environment; or Google Colab |
| **Notebook** | `notebooks/data_visualisation_lesson.ipynb` |

### Agenda

| Time | Section | Focus |
|------|---------|-------|
| 0:00 – 0:15 | Welcome & Pre-Class Review | Quiz on 3 pillars; resolve setup issues |
| 0:15 – 1:00 | Part 1: Data Storytelling | Conceptual — perception, design, 3-act structure |
| 1:00 – 1:05 | Break | — |
| 1:05 – 1:55 | Part 2: Matplotlib Fundamentals | Figure → Axes → Plot; customisation |
| 1:55 – 2:00 | Break | — |
| 2:00 – 2:55 | Part 3: Seaborn for Statistical Graphics | Distribution, categorical, and relational plots |
| 2:55 – 3:00 | Wrap-Up | Key Takeaways & Post-Class Assignment Briefing |

---

## 🏃 Part 1: Data Storytelling (45 min)

### 🎯 Learning Objective
Apply the three storytelling pillars (Perception, Design, Storytelling) to evaluate existing visualisations and structure a data narrative using the three-act framework.

### 📖 Theory Recap (15 min)

**Analogy:** A data visualisation without a story is like a movie without a script — technically filmed, but nobody knows what happened or why it matters.

**The Three Pillars:**

| Pillar | What it governs | Key principle |
|--------|----------------|---------------|
| **Perception** | How quickly viewers extract meaning | Pre-attentive processing: humans detect position, colour, and size in 200ms |
| **Design** | How you encode information visually | Match chart type to the analytical question |
| **Storytelling** | Why the audience should care | Three-act structure: Context → Problem → Action |

**The Three-Act Structure for Data:**
- **Act 1 — Context:** "Here's the world as it was." (Baseline, trend, expected state)
- **Act 2 — Problem/Opportunity:** "Here's what changed or what we discovered." (The finding)
- **Act 3 — Action:** "Here's what we recommend." (The business decision)

**Chart Selection Guide:**
| Question type | Recommended chart |
|--------------|-----------------|
| Comparison (categories) | Bar chart |
| Trend over time | Line chart |
| Part-to-whole | Pie chart / stacked bar |
| Distribution (single variable) | Histogram |
| Relationship (two variables) | Scatter plot |
| Distribution across categories | Box plot |

### 🛠️ Activity: "The Chart Critique" (20 min)

Show 3 real visualisations (or examples from the slides). For each, ask:
1. What is the message? Can you identify it in under 5 seconds?
2. Which pillar(s) is it violating?
3. How would you fix it?

**Common violations to spot:**
- Bar chart that doesn't start at zero → violation of Design (magnitude distortion)
- Pie chart with 8+ slices → violation of Perception (too many categories to process)
- Chart with no title or axis labels → violation of Storytelling (no context)
- Rainbow of colours with no meaning → violation of Design (colour overload)

**Discussion Questions:**
- "For an executive audience, which act of the 3-act structure is most important?"
- "Why should you always start a bar chart's Y-axis at zero?"

### 💬 Q&A & Reflection (10 min)

- **Common Misconception:** "Prettier charts are more effective." → Simplicity is more effective. Every extra element competes for attention. The best charts show one clear finding with minimal visual noise.
- **Business Case:** McKinsey's research shows that executives make faster, more confident decisions when data is presented with a clear 3-act narrative vs. raw data dumps. Data storytelling is a career skill, not just a technical one.

---

## 🏃 Part 2: Matplotlib Fundamentals (50 min)

### 🎯 Learning Objective
Create and customise charts using Matplotlib's Figure-Axes-Plot hierarchy, and produce publication-quality visualisations with proper labels, titles, and styling.

### 📖 Theory Recap (10 min)

**Analogy:** Matplotlib is like a painting studio.
- **Figure** = the canvas (the overall image)
- **Axes** = a framed painting on the canvas (one plot area)
- **Plot** = the actual marks on the painting (lines, bars, dots)

You can have **multiple Axes on one Figure** (subplots).

```python
import matplotlib.pyplot as plt

# The explicit OOP approach (recommended)
fig, ax = plt.subplots(figsize=(10, 6))
ax.plot(x, y, color='steelblue', linewidth=2, label='Revenue')
ax.set_title('Monthly Revenue 2023', fontsize=14)
ax.set_xlabel('Month')
ax.set_ylabel('Revenue (£)')
ax.legend()
plt.tight_layout()
plt.show()
```

**Always use the OOP interface** (`fig, ax = plt.subplots()`) rather than `plt.plot()` — it's cleaner when you have multiple plots.

### 🛠️ Hands-On Activity: "The Revenue Story" (30 min)

**In the notebook, code alongside:**

1. Simple line chart with customisation:
```python
import numpy as np
import matplotlib.pyplot as plt

months = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun']
revenue = [45000, 52000, 48000, 61000, 58000, 73000]
target = [50000] * 6

fig, ax = plt.subplots(figsize=(10, 5))
ax.plot(months, revenue, marker='o', color='steelblue', linewidth=2, label='Actual')
ax.plot(months, target, linestyle='--', color='red', linewidth=1, label='Target')
ax.fill_between(months, revenue, target, alpha=0.1, color='steelblue')
ax.set_title('H1 Revenue vs. Target', fontsize=14, fontweight='bold')
ax.set_ylabel('Revenue (£)')
ax.legend()
ax.grid(axis='y', alpha=0.3)
plt.tight_layout()
```

2. Bar chart with annotations:
```python
fig, ax = plt.subplots(figsize=(10, 5))
bars = ax.bar(months, revenue, color='steelblue', edgecolor='white')
ax.bar_label(bars, fmt='£{:,.0f}', padding=3, fontsize=9)
ax.set_title('Monthly Revenue — H1 2023')
ax.set_ylim(0, max(revenue) * 1.15)
```

3. Subplots — side by side:
```python
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(14, 5))
ax1.plot(months, revenue)
ax1.set_title('Line Chart')
ax2.bar(months, revenue)
ax2.set_title('Bar Chart')
plt.suptitle('Revenue — Two Views', fontsize=14)
plt.tight_layout()
```

**Discussion Questions:**
- "When would you use a line chart vs. a bar chart for monthly data?"
- "What does `plt.tight_layout()` fix?"

### 💬 Q&A & Reflection (10 min)

- **Common Misconception:** "I should use `plt.plot()` directly — it's simpler." → Fine for one quick chart. But `fig, ax = plt.subplots()` is the professional approach that scales to multiple subplots, saving formats, and complex customisation.
- **Business Case:** Bloomberg Terminal charts, the standard for financial professionals, are built on principles identical to what you've just implemented — clear axes, annotated key values, and minimal decoration.

---

## 🏃 Part 3: Seaborn for Statistical Graphics (50 min)

### 🎯 Learning Objective
Use Seaborn to create distribution, categorical, and relational plots that expose statistical patterns in data.

### 📖 Theory Recap (10 min)

**Analogy:** If Matplotlib is the painting studio, Seaborn is the artist's toolkit of brushes optimised for data — each brush (plot type) is pre-built for a specific statistical question.

Seaborn's advantage: it automatically computes statistical summaries (means, confidence intervals, distributions) from raw data — you don't pre-aggregate.

**Key plot types:**

| Question | Seaborn function |
|---------|-----------------|
| How is one variable distributed? | `sns.histplot()`, `sns.kdeplot()` |
| How do distributions compare across groups? | `sns.boxplot()`, `sns.violinplot()` |
| What's the relationship between two variables? | `sns.scatterplot()`, `sns.regplot()` |
| How does a metric vary across categories? | `sns.barplot()`, `sns.pointplot()` |
| What's the correlation matrix? | `sns.heatmap(df.corr())` |

Seaborn integrates directly with Pandas DataFrames: `sns.boxplot(data=df, x='category', y='value')`

### 🛠️ Hands-On Activity: "The Tips Dataset Report" (30 min)

**In the notebook, using the built-in tips dataset:**

```python
import seaborn as sns
tips = sns.load_dataset('tips')
```

1. Distribution of total bills:
```python
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 4))
sns.histplot(tips['total_bill'], bins=20, kde=True, ax=ax1)
sns.boxplot(y=tips['total_bill'], ax=ax2)
ax1.set_title('Distribution of Bill Amounts')
ax2.set_title('Box Plot of Bills')
plt.tight_layout()
```

2. Compare tips by day and time:
```python
fig, ax = plt.subplots(figsize=(10, 5))
sns.barplot(data=tips, x='day', y='tip', hue='time', ax=ax, capsize=0.1)
ax.set_title('Average Tip by Day and Meal Time')
ax.set_ylabel('Average Tip (£)')
```

3. Scatter with regression:
```python
sns.regplot(data=tips, x='total_bill', y='tip', scatter_kws={'alpha': 0.4})
plt.title('Tip vs. Bill Amount (with trend line)')
```

4. Correlation heatmap:
```python
numeric_cols = tips.select_dtypes(include='number')
fig, ax = plt.subplots(figsize=(6, 4))
sns.heatmap(numeric_cols.corr(), annot=True, fmt='.2f', cmap='coolwarm', ax=ax)
ax.set_title('Correlation Matrix')
```

**Discussion Questions:**
- "From the box plot, are there any outlier bills? How does Seaborn define an outlier?"
- "What does a correlation of 0.67 between `total_bill` and `tip` tell you?"

### 💬 Q&A & Reflection (10 min)

- **Common Misconception:** "Seaborn replaces Matplotlib." → Seaborn is built *on top of* Matplotlib. Use Seaborn for statistical plots and Matplotlib for fine-grained customisation. They work best together.
- **Business Case:** The UK's Office for National Statistics uses exactly this combination — Seaborn for exploratory analysis and Matplotlib for final publication-quality charts in their quarterly economic reports.

---

## 🎯 Wrap-Up (5 min)

### Key Takeaways
1. **Visualisation is communication** — choose your chart type based on what question you're answering and who your audience is.
2. **Matplotlib controls the canvas; Seaborn handles statistical plots** — use them together: `fig, ax = plt.subplots()` then `sns.barplot(..., ax=ax)`.
3. **Every chart needs a story** — title, axis labels, annotations, and a clear narrative are not decorations; they're the point.

### Next Steps
- **Post-Class:** Complete [post-class.md](./post-class.md) — chart critique, redesign challenge, self-assessment quiz, and reflection (2–3 hours).
- **Congratulations!** This is the final lesson of Module 1. You've covered the full data stack from database design to visualisation and storytelling.
