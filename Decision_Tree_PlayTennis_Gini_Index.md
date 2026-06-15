# Decision Tree Analysis: Play Tennis Dataset using GINI INDEX

## Complete Step-by-Step Guide

---

## Table of Contents
1. [Dataset Overview](#dataset-overview)
2. [Gini Index Explanation](#gini-index-explanation)
3. [Gini Index Calculation for Each Attribute](#gini-index-calculation-for-each-attribute)
4. [Building the Decision Tree with Gini](#building-the-decision-tree-with-gini)
5. [Final Tree and Decision Rules](#final-tree-and-decision-rules)
6. [Verification](#verification)
7. [Comparison: Entropy vs Gini Index](#comparison-entropy-vs-gini-index)

---

## Dataset Overview

The Play Tennis dataset is used here to build a decision tree using **Gini Index** instead of Entropy and Information Gain.

### Dataset Table

| Day | Outlook | Temperature | Humidity | Wind | PlayTennis |
|-----|---------|-------------|----------|------|-----------|
| D1 | Sunny | Hot | High | Weak | No |
| D2 | Sunny | Hot | High | Strong | No |
| D3 | Overcast | Hot | High | Weak | Yes |
| D4 | Rain | Mild | High | Weak | Yes |
| D5 | Rain | Cool | Normal | Weak | Yes |
| D6 | Rain | Cool | Normal | Strong | No |
| D7 | Overcast | Cool | Normal | Strong | Yes |
| D8 | Sunny | Mild | High | Weak | No |
| D9 | Sunny | Cool | Normal | Weak | Yes |
| D10 | Rain | Mild | Normal | Weak | Yes |
| D11 | Sunny | Mild | Normal | Strong | Yes |
| D12 | Overcast | Mild | High | Strong | Yes |
| D13 | Overcast | Hot | Normal | Weak | Yes |
| D14 | Rain | Mild | High | Strong | No |

### Data Summary
- **Total Samples**: 14 days
- **Target Variable**: PlayTennis (Yes/No)
- **Features**: 4 attributes (Outlook, Temperature, Humidity, Wind)
- **Yes Count**: 9 days
- **No Count**: 5 days

---

## Gini Index Explanation

### What is Gini Index?

Gini Index measures the **probability of incorrectly classifying a randomly chosen element** if it were randomly labeled according to the distribution of labels in the subset.

- **Low Gini** = More pure (better for splitting)
- **High Gini** = More mixed (worse)
- **Range**: 0 to 0.5 (for binary classification)
- **Gini = 0**: Perfectly pure
- **Gini = 0.5**: Maximum impurity (equal distribution)

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

## Gini Index Calculation for Each Attribute

### Initial Gini (Root Node)

**PlayTennis distribution:**
- Yes: 9/14 = 0.6429
- No: 5/14 = 0.3571

```
Gini(S) = 1 - (0.6429² + 0.3571²)
        = 1 - (0.4133 + 0.1275)
        = 1 - 0.5408
        = 0.4592
```

**Result: Initial Gini = 0.4592** (Moderate impurity)

---

## OUTLOOK Attribute

### Sunny Days: D1, D2, D8, D9, D11 (5 days)
- Yes: 2 (D9, D11)
- No: 3 (D1, D2, D8)

```
p_yes = 2/5 = 0.4
p_no = 3/5 = 0.6

Gini(Sunny) = 1 - (0.4² + 0.6²)
            = 1 - (0.16 + 0.36)
            = 1 - 0.52
            = 0.48
```

### Overcast Days: D3, D7, D12, D13 (4 days)
- Yes: 4 (D3, D7, D12, D13)
- No: 0

```
p_yes = 4/4 = 1.0
p_no = 0/4 = 0

Gini(Overcast) = 1 - (1.0² + 0²)
               = 1 - 1
               = 0 (PURE - All Yes)
```

### Rain Days: D4, D5, D6, D10, D14 (5 days)
- Yes: 3 (D4, D5, D10)
- No: 2 (D6, D14)

```
p_yes = 3/5 = 0.6
p_no = 2/5 = 0.4

Gini(Rain) = 1 - (0.6² + 0.4²)
           = 1 - (0.36 + 0.16)
           = 1 - 0.52
           = 0.48
```

### Weighted Average Gini

```
Gini(Outlook) = (5/14 × 0.48) + (4/14 × 0) + (5/14 × 0.48)
              = 0.1714 + 0 + 0.1714
              = 0.3428
```

### Gini Gain (Outlook)

```
Gini_Gain(Outlook) = 0.4592 - 0.3428
                   = 0.1164
```

---

## TEMPERATURE Attribute

### Hot Days: D1, D2, D3, D13 (4 days)
- Yes: 2 (D3, D13)
- No: 2 (D1, D2)

```
p_yes = 2/4 = 0.5
p_no = 2/4 = 0.5

Gini(Hot) = 1 - (0.5² + 0.5²)
          = 1 - (0.25 + 0.25)
          = 1 - 0.5
          = 0.5 (Maximum impurity)
```

### Mild Days: D4, D8, D10, D11, D12, D14 (6 days)
- Yes: 4 (D4, D10, D11, D12)
- No: 2 (D8, D14)

```
p_yes = 4/6 = 0.6667
p_no = 2/6 = 0.3333

Gini(Mild) = 1 - (0.6667² + 0.3333²)
           = 1 - (0.4444 + 0.1111)
           = 1 - 0.5555
           = 0.4445
```

### Cool Days: D5, D6, D7, D9 (4 days)
- Yes: 3 (D5, D7, D9)
- No: 1 (D6)

```
p_yes = 3/4 = 0.75
p_no = 1/4 = 0.25

Gini(Cool) = 1 - (0.75² + 0.25²)
           = 1 - (0.5625 + 0.0625)
           = 1 - 0.625
           = 0.375
```

### Weighted Average Gini

```
Gini(Temperature) = (4/14 × 0.5) + (6/14 × 0.4445) + (4/14 × 0.375)
                  = 0.1429 + 0.1905 + 0.1071
                  = 0.4405
```

### Gini Gain (Temperature)

```
Gini_Gain(Temperature) = 0.4592 - 0.4405
                       = 0.0187
```

---

## HUMIDITY Attribute

### High Days: D1, D2, D3, D4, D8, D12, D14 (7 days)
- Yes: 3 (D3, D4, D12)
- No: 4 (D1, D2, D8, D14)

```
p_yes = 3/7 = 0.4286
p_no = 4/7 = 0.5714

Gini(High) = 1 - (0.4286² + 0.5714²)
           = 1 - (0.1837 + 0.3265)
           = 1 - 0.5102
           = 0.4898
```

### Normal Days: D5, D6, D7, D9, D10, D11, D13 (7 days)
- Yes: 6 (D5, D7, D9, D10, D11, D13)
- No: 1 (D6)

```
p_yes = 6/7 = 0.8571
p_no = 1/7 = 0.1429

Gini(Normal) = 1 - (0.8571² + 0.1429²)
             = 1 - (0.7346 + 0.0204)
             = 1 - 0.7550
             = 0.2450
```

### Weighted Average Gini

```
Gini(Humidity) = (7/14 × 0.4898) + (7/14 × 0.2450)
               = 0.2449 + 0.1225
               = 0.3674
```

### Gini Gain (Humidity)

```
Gini_Gain(Humidity) = 0.4592 - 0.3674
                    = 0.0918
```

---

## WIND Attribute

### Weak Days: D1, D3, D4, D5, D8, D9, D10, D13 (8 days)
- Yes: 6 (D3, D4, D5, D9, D10, D13)
- No: 2 (D1, D8)

```
p_yes = 6/8 = 0.75
p_no = 2/8 = 0.25

Gini(Weak) = 1 - (0.75² + 0.25²)
           = 1 - (0.5625 + 0.0625)
           = 1 - 0.625
           = 0.375
```

### Strong Days: D2, D6, D7, D11, D12, D14 (6 days)
- Yes: 3 (D7, D11, D12)
- No: 3 (D2, D6, D14)

```
p_yes = 3/6 = 0.5
p_no = 3/6 = 0.5

Gini(Strong) = 1 - (0.5² + 0.5²)
             = 1 - (0.25 + 0.25)
             = 1 - 0.5
             = 0.5
```

### Weighted Average Gini

```
Gini(Wind) = (8/14 × 0.375) + (6/14 × 0.5)
           = 0.2143 + 0.2143
           = 0.4286
```

### Gini Gain (Wind)

```
Gini_Gain(Wind) = 0.4592 - 0.4286
                = 0.0306
```

---

## Gini Gain Comparison (Level 1)

| Attribute | Gini Index | Gini Gain | Ranking |
|-----------|-----------|-----------|---------|
| **Outlook** | 0.3428 | **0.1164** | 🥇 BEST |
| Humidity | 0.3674 | 0.0918 | 🥈 2nd |
| Wind | 0.4286 | 0.0306 | 🥉 3rd |
| Temperature | 0.4405 | 0.0187 | 4th |

**🎯 Decision: ROOT NODE = OUTLOOK** (Same as Entropy!)

---

## Building the Decision Tree with Gini

### Branch 1: OUTLOOK = OVERCAST

**Overcast Days:** D3, D7, D12, D13
- All are PlayTennis = Yes
- Gini = 0 (PURE)

**✅ LEAF NODE: PlayTennis = YES**

---

### Branch 2: OUTLOOK = SUNNY

**Sunny Days:** D1, D2, D8, D9, D11 (5 days)
- Yes: 2, No: 3
- Gini = 0.48 (MIXED - Need to split)

#### Finding Best Split for Sunny Branch

**Testing HUMIDITY on Sunny Days:**

**High (Sunny):** D1, D2, D8 (3 days)
- Yes: 0, No: 3

```
Gini(Sunny-High) = 1 - (0² + 1²)
                 = 0 (PURE - All No)
```

**Normal (Sunny):** D9, D11 (2 days)
- Yes: 2, No: 0

```
Gini(Sunny-Normal) = 1 - (1² + 0²)
                   = 0 (PURE - All Yes)
```

**Weighted Average:**
```
Gini(Sunny-Humidity) = (3/5 × 0) + (2/5 × 0)
                     = 0 (PERFECT!)
```

**Gini Gain (Sunny-Humidity):**
```
Gini_Gain = 0.48 - 0 = 0.48 ⭐ BEST
```

---

**Testing TEMPERATURE on Sunny Days:**

**Hot (Sunny):** D1, D2 (2 days)
- Yes: 0, No: 2
- Gini = 0

**Mild (Sunny):** D8, D11 (2 days)
- Yes: 1, No: 1
- Gini = 0.5

**Cool (Sunny):** D9 (1 day)
- Yes: 1, No: 0
- Gini = 0

```
Gini(Sunny-Temp) = (2/5 × 0) + (2/5 × 0.5) + (1/5 × 0)
                 = 0.2
Gini_Gain = 0.48 - 0.2 = 0.28
```

---

**Testing WIND on Sunny Days:**

**Weak (Sunny):** D1, D8, D9 (3 days)
- Yes: 1, No: 2
- Gini = 1 - (0.333² + 0.667²) = 0.444

**Strong (Sunny):** D2, D11 (2 days)
- Yes: 1, No: 1
- Gini = 0.5

```
Gini(Sunny-Wind) = (3/5 × 0.444) + (2/5 × 0.5)
                 = 0.2664 + 0.2
                 = 0.4664
Gini_Gain = 0.48 - 0.4664 = 0.0136
```

---

**Best Split for Sunny: HUMIDITY (Gini Gain = 0.48)**

**✅ Split Sunny on HUMIDITY:**
- Sunny + High → PlayTennis = **No**
- Sunny + Normal → PlayTennis = **Yes**

---

### Branch 3: OUTLOOK = RAIN

**Rain Days:** D4, D5, D6, D10, D14 (5 days)
- Yes: 3, No: 2
- Gini = 0.48 (MIXED - Need to split)

#### Finding Best Split for Rain Branch

**Testing WIND on Rain Days:**

**Weak (Rain):** D4, D5, D10 (3 days)
- Yes: 3, No: 0

```
Gini(Rain-Weak) = 1 - (1² + 0²)
                = 0 (PURE - All Yes)
```

**Strong (Rain):** D6, D14 (2 days)
- Yes: 0, No: 2

```
Gini(Rain-Strong) = 1 - (0² + 1²)
                  = 0 (PURE - All No)
```

**Weighted Average:**
```
Gini(Rain-Wind) = (3/5 × 0) + (2/5 × 0)
                = 0 (PERFECT!)
```

**Gini Gain (Rain-Wind):**
```
Gini_Gain = 0.48 - 0 = 0.48 ⭐ BEST
```

---

**Testing HUMIDITY on Rain Days:**

**High (Rain):** D4, D14 (2 days)
- Yes: 1, No: 1
- Gini = 0.5

**Normal (Rain):** D5, D6, D10 (3 days)
- Yes: 2, No: 1
- Gini = 1 - (0.667² + 0.333²) = 0.444

```
Gini(Rain-Humidity) = (2/5 × 0.5) + (3/5 × 0.444)
                    = 0.2 + 0.2664
                    = 0.4664
Gini_Gain = 0.48 - 0.4664 = 0.0136
```

---

**Testing TEMPERATURE on Rain Days:**

**Mild (Rain):** D4, D10, D14 (3 days)
- Yes: 2, No: 1
- Gini = 0.444

**Cool (Rain):** D5, D6 (2 days)
- Yes: 1, No: 1
- Gini = 0.5

```
Gini(Rain-Temp) = (3/5 × 0.444) + (2/5 × 0.5)
                = 0.2664 + 0.2
                = 0.4664
Gini_Gain = 0.48 - 0.4664 = 0.0136
```

---

**Best Split for Rain: WIND (Gini Gain = 0.48)**

**✅ Split Rain on WIND:**
- Rain + Weak → PlayTennis = **Yes**
- Rain + Strong → PlayTennis = **No**

---

## Final Tree and Decision Rules

### Decision Tree Structure (Using Gini Index)

```
                          Outlook
                        /    |    \
                       /     |     \
                   Sunny   Overcast  Rain
                    |         |       |
                 Humidity     Yes    Wind
                 /    \             /  \
               High  Normal        Weak Strong
               |       |            |    |
               No      Yes          Yes   No
```

### Decision Rules

1. **If Outlook = Overcast** → **PlayTennis = YES** ✅
2. **If Outlook = Sunny AND Humidity = High** → **PlayTennis = NO** ❌
3. **If Outlook = Sunny AND Humidity = Normal** → **PlayTennis = YES** ✅
4. **If Outlook = Rain AND Wind = Weak** → **PlayTennis = YES** ✅
5. **If Outlook = Rain AND Wind = Strong** → **PlayTennis = NO** ❌

---

## Verification

### Testing Decision Tree on All 14 Days

| Day | Outlook | Humidity/Wind | Prediction | Actual | ✓ |
|-----|---------|---------------|-----------|--------|---|
| D1 | Sunny | High | **No** | No | ✓ |
| D2 | Sunny | High | **No** | No | ✓ |
| D3 | Overcast | - | **Yes** | Yes | ✓ |
| D4 | Rain | Weak | **Yes** | Yes | ✓ |
| D5 | Rain | Weak | **Yes** | Yes | ✓ |
| D6 | Rain | Strong | **No** | No | ✓ |
| D7 | Overcast | - | **Yes** | Yes | ✓ |
| D8 | Sunny | High | **No** | No | ✓ |
| D9 | Sunny | Normal | **Yes** | Yes | ✓ |
| D10 | Rain | Weak | **Yes** | Yes | ✓ |
| D11 | Sunny | Normal | **Yes** | Yes | ✓ |
| D12 | Overcast | - | **Yes** | Yes | ✓ |
| D13 | Overcast | - | **Yes** | Yes | ✓ |
| D14 | Rain | Strong | **No** | No | ✓ |

**🎯 Accuracy: 14/14 = 100%** ✨

---

## Comparison: Entropy vs Gini Index

### Side-by-Side Comparison of Key Metrics

| Metric | Entropy | Gini Index |
|--------|---------|-----------|
| **Initial Impurity** | 0.9387 bits | 0.4592 |
| **Outlook IG/Gain** | 0.2451 | 0.1164 |
| **Humidity IG/Gain** | 0.1516 | 0.0918 |
| **Wind IG/Gain** | 0.0466 | 0.0306 |
| **Temperature IG/Gain** | 0.0277 | 0.0187 |
| **Best Split (Level 1)** | Outlook | Outlook ✓ |
| **Sunny-Humidity Gain** | 0.9710 | 0.48 |
| **Rain-Wind Gain** | 0.9710 | 0.48 |

### Key Observations

1. **Same Root Node**: Both methods choose **Outlook** as the root node ✓
2. **Same Final Tree**: Both produce identical tree structure ✓
3. **Different Magnitudes**: Entropy values are larger (logarithmic scale) vs Gini (squared probabilities)
4. **Rank Order**: Both methods rank attributes in the same order
5. **Final Accuracy**: Both achieve 100% accuracy on training data

### Why Both Methods Work?

- **Entropy** uses logarithmic calculations → Different scale but same ranking
- **Gini Index** uses probability squares → Simpler computation, same ranking
- For most datasets, they produce similar or identical trees
- Gini Index is faster computationally (no logarithms)

---

## Advantages and Disadvantages

### Entropy (Information Gain)

**Advantages:**
- Well-established information theory foundation
- More intuitive (measuring disorder/uncertainty)
- Detailed probabilistic interpretation

**Disadvantages:**
- Computationally slower (requires logarithms)
- Slightly biased toward attributes with more values

### Gini Index

**Advantages:**
- Computationally efficient (simple arithmetic)
- Easier to understand (probability-based)
- Less biased toward high-cardinality attributes
- Default in many libraries (scikit-learn, CART algorithm)

**Disadvantages:**
- Less theoretical foundation
- Similar results to entropy (not always obvious why)

---

## Summary

### Decision Tree Results

| Characteristic | Value |
|---|---|
| Root Node | Outlook |
| Depth | 2 levels |
| Number of Leaves | 5 |
| Decision Rules | 5 |
| Training Accuracy | 100% (14/14) |
| Splitting Criterion | Gini Index |
| Algorithm | CART (Classification and Regression Trees) |

### Conclusion

The **Gini Index method produces the same decision tree** as the Entropy method for the Play Tennis dataset. This demonstrates that while the mathematical approaches differ, both impurity measures effectively identify the same optimal splitting features. The Gini Index achieves this with simpler, faster computations, making it a practical choice for real-world applications.

---

**Generated for ML Course - Decision Tree Analysis using Gini Index**
*Date: June 15, 2026*
