# Decision Tree Analysis: Activity Dataset using GINI INDEX

## Complete Step-by-Step Guide

---

## Table of Contents
1. [Dataset Overview](#dataset-overview)
2. [Gini Index Explanation](#gini-index-explanation)
3. [Initial Gini Calculation](#initial-gini-calculation)
4. [Gini Index for Each Attribute](#gini-index-for-each-attribute)
5. [Building the Decision Tree with Gini](#building-the-decision-tree-with-gini)
6. [Final Tree and Decision Rules](#final-tree-and-decision-rules)
7. [Verification](#verification)

---

## Dataset Overview

The Activity dataset contains information about 10 people with their characteristics and their activity preference.

### Dataset Table

| Person | Weather | Job | Money | Activity |
|--------|---------|-----|-------|----------|
| P1 | Sunny | Yes | Rich | Cinema |
| P2 | Sunny | No | Rich | Tennis |
| P3 | Windy | Yes | Rich | Cinema |
| P4 | Rainy | Yes | Poor | Cinema |
| P5 | Rainy | No | Rich | Stay In |
| P6 | Rainy | Yes | Poor | Cinema |
| P7 | Windy | No | Poor | Cinema |
| P8 | Windy | No | Rich | Shopping |
| P9 | Windy | Yes | Rich | Cinema |
| P10 | Sunny | No | Rich | Tennis |

### Data Summary
- **Total Samples**: 10 people
- **Target Variable**: Activity (Cinema, Tennis, Stay In, Shopping)
- **Features**: 3 attributes (Weather, Job, Money)
- **Activity Distribution**:
  - Cinema: 6 (P1, P3, P4, P6, P7, P9)
  - Tennis: 2 (P2, P10)
  - Stay In: 1 (P5)
  - Shopping: 1 (P8)

---

## Gini Index Explanation

### What is Gini Index?

Gini Index measures the **probability of incorrectly classifying a randomly chosen element** if it were randomly labeled according to the distribution of labels in the subset.

- **Low Gini** = More pure (better for splitting)
- **High Gini** = More mixed (worse)
- **Range**: 0 to 0.75 (for multiclass - 4 classes here)
- **Gini = 0**: Perfectly pure
- **Gini = 0.75**: Maximum impurity (equal distribution in 4 classes)

### Formula

```
Gini(S) = 1 - Σ (p_i)²

Where:
- S = Dataset
- p_i = Proportion of class i
```

### Gini Gain (Information Gain with Gini)

```
Gini_Gain = Gini(Parent) - Weighted_Average_Gini(Children)
```

---

## Initial Gini Calculation

### Activity Distribution

- Cinema: 6/10 = 0.6
- Tennis: 2/10 = 0.2
- Stay In: 1/10 = 0.1
- Shopping: 1/10 = 0.1

### Root Node Gini

```
Gini(S) = 1 - (p_cinema² + p_tennis² + p_stayIn² + p_shopping²)
        = 1 - (0.6² + 0.2² + 0.1² + 0.1²)
        = 1 - (0.36 + 0.04 + 0.01 + 0.01)
        = 1 - 0.42
        = 0.58
```

**Result: Initial Gini = 0.58** (Moderate impurity - 4 classes mixed)

---

## Gini Index for Each Attribute

---

## WEATHER Attribute

### Sunny Days: P1, P2, P10 (3 people)
- Cinema: 1 (P1)
- Tennis: 2 (P2, P10)
- Stay In: 0
- Shopping: 0

```
p_cinema = 1/3 = 0.3333
p_tennis = 2/3 = 0.6667
p_stayIn = 0/3 = 0
p_shopping = 0/3 = 0

Gini(Sunny) = 1 - (0.3333² + 0.6667² + 0² + 0²)
            = 1 - (0.1111 + 0.4444 + 0 + 0)
            = 1 - 0.5555
            = 0.4445
```

### Windy Days: P3, P7, P8, P9 (4 people)
- Cinema: 3 (P3, P7, P9)
- Tennis: 0
- Stay In: 0
- Shopping: 1 (P8)

```
p_cinema = 3/4 = 0.75
p_tennis = 0/4 = 0
p_stayIn = 0/4 = 0
p_shopping = 1/4 = 0.25

Gini(Windy) = 1 - (0.75² + 0² + 0² + 0.25²)
            = 1 - (0.5625 + 0 + 0 + 0.0625)
            = 1 - 0.625
            = 0.375
```

### Rainy Days: P4, P5, P6 (3 people)
- Cinema: 2 (P4, P6)
- Tennis: 0
- Stay In: 1 (P5)
- Shopping: 0

```
p_cinema = 2/3 = 0.6667
p_tennis = 0/3 = 0
p_stayIn = 1/3 = 0.3333
p_shopping = 0/3 = 0

Gini(Rainy) = 1 - (0.6667² + 0² + 0.3333² + 0²)
            = 1 - (0.4444 + 0 + 0.1111 + 0)
            = 1 - 0.5555
            = 0.4445
```

### Weighted Average Gini for Weather

```
Gini(Weather) = (3/10 × 0.4445) + (4/10 × 0.375) + (3/10 × 0.4445)
              = 0.1334 + 0.15 + 0.1334
              = 0.4168
```

### Gini Gain (Weather)

```
Gini_Gain(Weather) = 0.58 - 0.4168
                   = 0.1632
```

---

## JOB Attribute

### Job = Yes: P1, P3, P4, P6, P9 (5 people)
- Cinema: 5 (P1, P3, P4, P6, P9)
- Tennis: 0
- Stay In: 0
- Shopping: 0

```
p_cinema = 5/5 = 1.0
p_tennis = 0/5 = 0
p_stayIn = 0/5 = 0
p_shopping = 0/5 = 0

Gini(Job=Yes) = 1 - (1.0² + 0² + 0² + 0²)
              = 1 - 1
              = 0 (PURE - All Cinema!)
```

### Job = No: P2, P5, P7, P8, P10 (5 people)
- Cinema: 1 (P7)
- Tennis: 2 (P2, P10)
- Stay In: 1 (P5)
- Shopping: 1 (P8)

```
p_cinema = 1/5 = 0.2
p_tennis = 2/5 = 0.4
p_stayIn = 1/5 = 0.2
p_shopping = 1/5 = 0.2

Gini(Job=No) = 1 - (0.2² + 0.4² + 0.2² + 0.2²)
             = 1 - (0.04 + 0.16 + 0.04 + 0.04)
             = 1 - 0.28
             = 0.72
```

### Weighted Average Gini for Job

```
Gini(Job) = (5/10 × 0) + (5/10 × 0.72)
          = 0 + 0.36
          = 0.36
```

### Gini Gain (Job)

```
Gini_Gain(Job) = 0.58 - 0.36
               = 0.22 ⭐ BEST SO FAR!
```

---

## MONEY Attribute

### Money = Rich: P1, P2, P3, P8, P9, P10 (6 people)
- Cinema: 2 (P1, P3, P9)
- Tennis: 2 (P2, P10)
- Stay In: 0
- Shopping: 1 (P8)

```
p_cinema = 3/6 = 0.5
p_tennis = 2/6 = 0.3333
p_stayIn = 0/6 = 0
p_shopping = 1/6 = 0.1667

Gini(Money=Rich) = 1 - (0.5² + 0.3333² + 0² + 0.1667²)
                 = 1 - (0.25 + 0.1111 + 0 + 0.0278)
                 = 1 - 0.3889
                 = 0.6111
```

### Money = Poor: P4, P5, P6, P7 (4 people)
- Cinema: 3 (P4, P6, P7)
- Tennis: 0
- Stay In: 1 (P5)
- Shopping: 0

```
p_cinema = 3/4 = 0.75
p_tennis = 0/4 = 0
p_stayIn = 1/4 = 0.25
p_shopping = 0/4 = 0

Gini(Money=Poor) = 1 - (0.75² + 0² + 0.25² + 0²)
                 = 1 - (0.5625 + 0 + 0.0625 + 0)
                 = 1 - 0.625
                 = 0.375
```

### Weighted Average Gini for Money

```
Gini(Money) = (6/10 × 0.6111) + (4/10 × 0.375)
            = 0.3667 + 0.15
            = 0.5167
```

### Gini Gain (Money)

```
Gini_Gain(Money) = 0.58 - 0.5167
                 = 0.0633
```

---

## Gini Gain Comparison (Level 1)

| Attribute | Gini Index | Gini Gain | Ranking |
|-----------|-----------|-----------|---------|
| **Job** | **0.36** | **0.22** | 🥇 BEST |
| Weather | 0.4168 | 0.1632 | 🥈 2nd |
| Money | 0.5167 | 0.0633 | 🥉 3rd |

**🎯 Decision: ROOT NODE = JOB** (Highest Gini Gain)

---

## Building the Decision Tree with Gini

### Branch 1: JOB = YES

**People with Job = Yes:** P1, P3, P4, P6, P9
- Activity: Cinema, Cinema, Cinema, Cinema, Cinema
- All 5 people have Activity = Cinema
- Gini = 0 (PURE)

**✅ LEAF NODE: Activity = CINEMA**

---

### Branch 2: JOB = NO

**People with Job = No:** P2, P5, P7, P8, P10 (5 people)
- Activities: Tennis (P2), Stay In (P5), Cinema (P7), Shopping (P8), Tennis (P10)
- Distribution: Cinema=1, Tennis=2, Stay In=1, Shopping=1
- Gini = 0.72 (MIXED - Need to split)

#### Finding Best Split for Job = No Branch

**Testing WEATHER on Job = No:**

**Sunny (Job=No):** P2, P10 (2 people)
- Activity: Tennis, Tennis
- All Tennis!

```
Gini(Sunny-JobNo) = 1 - (0² + 1² + 0² + 0²)
                  = 0 (PURE!)
```

**Windy (Job=No):** P7, P8 (2 people)
- Activity: Cinema (P7), Shopping (P8)
- Mixed: 1 Cinema, 1 Shopping

```
p_cinema = 1/2 = 0.5
p_shopping = 1/2 = 0.5
(Tennis and StayIn = 0)

Gini(Windy-JobNo) = 1 - (0.5² + 0² + 0² + 0.5²)
                  = 1 - (0.25 + 0 + 0 + 0.25)
                  = 1 - 0.5
                  = 0.5
```

**Rainy (Job=No):** P5 (1 person)
- Activity: Stay In
- Pure!

```
Gini(Rainy-JobNo) = 1 - (0² + 0² + 1² + 0²)
                  = 0 (PURE!)
```

### Weighted Average Gini for Weather (Job=No)

```
Gini(Weather|JobNo) = (2/5 × 0) + (2/5 × 0.5) + (1/5 × 0)
                    = 0 + 0.2 + 0
                    = 0.2
```

### Gini Gain (Weather on Job=No)

```
Gini_Gain = 0.72 - 0.2 = 0.52 ⭐ EXCELLENT!
```

---

**Testing MONEY on Job = No:**

**Rich (Job=No):** P2, P8, P10 (3 people)
- Activity: Tennis (P2), Shopping (P8), Tennis (P10)
- Distribution: Cinema=0, Tennis=2, Stay In=0, Shopping=1

```
p_tennis = 2/3 = 0.6667
p_shopping = 1/3 = 0.3333
(Cinema and StayIn = 0)

Gini(Rich-JobNo) = 1 - (0² + 0.6667² + 0² + 0.3333²)
                 = 1 - (0 + 0.4444 + 0 + 0.1111)
                 = 1 - 0.5555
                 = 0.4445
```

**Poor (Job=No):** P5, P7 (2 people)
- Activity: Stay In (P5), Cinema (P7)
- Distribution: Cinema=1, Tennis=0, Stay In=1, Shopping=0

```
p_cinema = 1/2 = 0.5
p_stayIn = 1/2 = 0.5

Gini(Poor-JobNo) = 1 - (0.5² + 0² + 0.5² + 0²)
                 = 1 - (0.25 + 0 + 0.25 + 0)
                 = 1 - 0.5
                 = 0.5
```

### Weighted Average Gini for Money (Job=No)

```
Gini(Money|JobNo) = (3/5 × 0.4445) + (2/5 × 0.5)
                  = 0.2667 + 0.2
                  = 0.4667
```

### Gini Gain (Money on Job=No)

```
Gini_Gain = 0.72 - 0.4667 = 0.2533
```

---

**Best Split for Job = No: WEATHER (Gini Gain = 0.52)**

**✅ Split Job = No on WEATHER:**
- Job = No + Sunny → Activity = **Tennis** (P2, P10)
- Job = No + Windy → Activity = Need further split (P7=Cinema, P8=Shopping)
- Job = No + Rainy → Activity = **Stay In** (P5)

---

### Sub-branch: JOB = NO & WEATHER = WINDY

**People:** P7, P8 (2 people)
- P7: Cinema
- P8: Shopping
- Gini = 0.5 (Still mixed)

#### Testing remaining attributes on this subset:

**MONEY for Windy-JobNo:**

**Rich (Windy-JobNo):** P8 (1 person)
- Activity: Shopping
- Gini = 0 (PURE)

**Poor (Windy-JobNo):** P7 (1 person)
- Activity: Cinema
- Gini = 0 (PURE)

```
Gini(Money|WindyJobNo) = (1/2 × 0) + (1/2 × 0) = 0 (PERFECT!)
Gini_Gain = 0.5 - 0 = 0.5
```

**✅ Split Windy-JobNo on MONEY:**
- Windy + JobNo + Rich → Activity = **Shopping** (P8)
- Windy + JobNo + Poor → Activity = **Cinema** (P7)

---

## Final Tree and Decision Rules

### Decision Tree Structure (Using Gini Index)

```
                            Job
                          /    \
                        Yes      No
                        |        |
                     Cinema    Weather
                              /  |  \
                          Sunny Windy Rainy
                            |     |     |
                          Tennis  Money  StayIn
                                 /  \
                              Rich  Poor
                              |     |
                           Shopping Cinema
```

### Decision Rules

1. **If Job = Yes** → **Activity = CINEMA** ✅
2. **If Job = No AND Weather = Sunny** → **Activity = TENNIS** ✅
3. **If Job = No AND Weather = Rainy** → **Activity = STAY IN** ✅
4. **If Job = No AND Weather = Windy AND Money = Rich** → **Activity = SHOPPING** ✅
5. **If Job = No AND Weather = Windy AND Money = Poor** → **Activity = CINEMA** ✅

---

## Verification

### Testing Decision Tree on All 10 People

| Person | Job | Weather | Money | Prediction | Actual | ✓ |
|--------|-----|---------|-------|-----------|--------|---|
| P1 | Yes | Sunny | Rich | **Cinema** | Cinema | ✓ |
| P2 | No | Sunny | Rich | **Tennis** | Tennis | ✓ |
| P3 | Yes | Windy | Rich | **Cinema** | Cinema | ✓ |
| P4 | Yes | Rainy | Poor | **Cinema** | Cinema | ✓ |
| P5 | No | Rainy | Rich | **Stay In** | Stay In | ✓ |
| P6 | Yes | Rainy | Poor | **Cinema** | Cinema | ✓ |
| P7 | No | Windy | Poor | **Cinema** | Cinema | ✓ |
| P8 | No | Windy | Rich | **Shopping** | Shopping | ✓ |
| P9 | Yes | Windy | Rich | **Cinema** | Cinema | ✓ |
| P10 | No | Sunny | Rich | **Tennis** | Tennis | ✓ |

**🎯 Accuracy: 10/10 = 100%** ✨

---

## Decision Tree Summary

### Tree Characteristics

| Characteristic | Value |
|---|---|
| Root Node | Job |
| Depth | 3 levels |
| Number of Leaf Nodes | 5 |
| Number of Decision Rules | 5 |
| Training Accuracy | 100% (10/10) |
| Splitting Criterion | Gini Index |
| Algorithm | CART (Classification and Regression Trees) |

### Attribute Importance (Level 1)

| Attribute | Gini Gain | Importance |
|-----------|-----------|-----------|
| Job | 0.22 | 🥇 Most Important |
| Weather | 0.1632 | 🥈 Important |
| Money | 0.0633 | 🥉 Less Important |

---

## Key Observations

1. **Job attribute is the strongest predictor** - It perfectly separates people with Job=Yes (all Cinema) from Job=No (mixed activities)

2. **Weather is the second-level splitter** - Among Job=No people, Weather perfectly separates Tennis (Sunny) and Stay In (Rainy)

3. **Money is tertiary** - Only needed to distinguish Cinema from Shopping in the Windy-JobNo group

4. **Perfect Classification** - The decision tree achieves 100% accuracy on the training data

5. **Clear Decision Rules** - All rules are interpretable and actionable

---

## Gini Index Insights

### Why Gini Index Works Well Here:

- **Multiclass Problem**: 4 different activities work well with Gini (range 0 to 0.75)
- **Clear Splits**: Some attributes create perfectly pure splits (Gini=0)
- **Computational Efficiency**: Simple arithmetic operations make it fast
- **Effective Impurity Measure**: Successfully ranks attributes by their discriminative power

---

**Generated for ML Course - Decision Tree Analysis using Gini Index**
*Dataset: Activity Prediction (10 Samples, 4 Classes)*
*Date: June 15, 2026*
