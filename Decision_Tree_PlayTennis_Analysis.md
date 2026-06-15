# Decision Tree Analysis: Play Tennis Dataset

## Complete Step-by-Step Guide

---

## Table of Contents
1. [Dataset Overview](#dataset-overview)
2. [Entropy Calculation](#entropy-calculation)
3. [Information Gain for Each Attribute](#information-gain-for-each-attribute)
4. [Building the Decision Tree](#building-the-decision-tree)
5. [Final Tree and Decision Rules](#final-tree-and-decision-rules)
6. [Verification](#verification)

---

## Dataset Overview

The Play Tennis dataset is a classic machine learning dataset used to teach decision trees. It contains 14 days of weather data and whether it was suitable to play tennis.

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

## Entropy Calculation

### What is Entropy?

Entropy measures **disorder or uncertainty** in a dataset. 
- **High Entropy** = Data is mixed (uncertain)
- **Low Entropy** = Data is pure (certain)
- **Maximum Entropy** = 1.0 (equal distribution)
- **Minimum Entropy** = 0 (completely pure)

### Formula

```
Entropy(S) = -Σ (p_i × log₂(p_i))

Where:
- S = Dataset
- p_i = Proportion of class i
- log₂ = Logarithm base 2
```

### Total Dataset Entropy

**Count PlayTennis outcomes:**
- PlayTennis = Yes: 9 days
- PlayTennis = No: 5 days
- Total: 14 days

**Calculation:**
```
p_yes = 9/14 ≈ 0.6429
p_no = 5/14 ≈ 0.3571

Entropy(S) = -(0.6429 × log₂(0.6429)) - (0.3571 × log₂(0.3571))
           = -(0.6429 × -0.6365) - (0.3571 × -1.4855)
           = 0.4087 + 0.5300
           = 0.9387 bits
```

**Result: Initial Entropy = 0.9387 bits** (High uncertainty)

---

## Information Gain for Each Attribute

### What is Information Gain?

Information Gain measures **how much entropy is reduced** after splitting on an attribute.

```
IG(Attribute) = Entropy(Before) - Entropy(After Split)
```

Higher IG = Better split!

---

## OUTLOOK Attribute

### Sunny Days: D1, D2, D8, D9, D11 (5 days)
- Yes: D9, D11 = 2
- No: D1, D2, D8 = 3

```
Entropy(Sunny) = -(2/5 × log₂(2/5)) - (3/5 × log₂(3/5))
               = -(0.4 × -1.3219) - (0.6 × -0.7370)
               = 0.5288 + 0.4422
               = 0.9710 bits
```

### Overcast Days: D3, D7, D12, D13 (4 days)
- Yes: D3, D7, D12, D13 = 4
- No: 0

```
Entropy(Overcast) = 0 bits (PURE - All Yes)
```

### Rain Days: D4, D5, D6, D10, D14 (5 days)
- Yes: D4, D5, D10 = 3
- No: D6, D14 = 2

```
Entropy(Rain) = -(3/5 × log₂(3/5)) - (2/5 × log₂(2/5))
              = 0.4422 + 0.5288
              = 0.9710 bits
```

### Weighted Average Entropy

```
Entropy(Outlook) = (5/14 × 0.9710) + (4/14 × 0) + (5/14 × 0.9710)
                 = 0.3468 + 0 + 0.3468
                 = 0.6936 bits
```

### Information Gain (Outlook)

```
IG(Outlook) = 0.9387 - 0.6936 = 0.2451 bits
```

---

## TEMPERATURE Attribute

### Hot Days: D1, D2, D3, D13 (4 days)
- Yes: D3, D13 = 2
- No: D1, D2 = 2

```
Entropy(Hot) = -(0.5 × log₂(0.5)) - (0.5 × log₂(0.5))
             = 0.5 + 0.5
             = 1.0 bits
```

### Mild Days: D4, D8, D10, D11, D12, D14 (6 days)
- Yes: D4, D10, D11, D12 = 4
- No: D8, D14 = 2

```
Entropy(Mild) = -(4/6 × log₂(4/6)) - (2/6 × log₂(2/6))
              = 0.3900 + 0.5283
              = 0.9183 bits
```

### Cool Days: D5, D6, D7, D9 (4 days)
- Yes: D5, D7, D9 = 3
- No: D6 = 1

```
Entropy(Cool) = -(3/4 × log₂(3/4)) - (1/4 × log₂(1/4))
              = 0.3112 + 0.5
              = 0.8112 bits
```

### Weighted Average Entropy

```
Entropy(Temperature) = (4/14 × 1.0) + (6/14 × 0.9183) + (4/14 × 0.8112)
                     = 0.2857 + 0.3935 + 0.2318
                     = 0.9110 bits
```

### Information Gain (Temperature)

```
IG(Temperature) = 0.9387 - 0.9110 = 0.0277 bits
```

---

## HUMIDITY Attribute

### High Days: D1, D2, D3, D4, D8, D12, D14 (7 days)
- Yes: D3, D4, D12 = 3
- No: D1, D2, D8, D14 = 4

```
Entropy(High) = -(3/7 × log₂(3/7)) - (4/7 × log₂(4/7))
              = 0.5229 + 0.4616
              = 0.9845 bits
```

### Normal Days: D5, D6, D7, D9, D10, D11, D13 (7 days)
- Yes: D5, D7, D9, D10, D11, D13 = 6
- No: D6 = 1

```
Entropy(Normal) = -(6/7 × log₂(6/7)) - (1/7 × log₂(1/7))
                = 0.1887 + 0.4010
                = 0.5897 bits
```

### Weighted Average Entropy

```
Entropy(Humidity) = (7/14 × 0.9845) + (7/14 × 0.5897)
                  = 0.4923 + 0.2948
                  = 0.7871 bits
```

### Information Gain (Humidity)

```
IG(Humidity) = 0.9387 - 0.7871 = 0.1516 bits
```

---

## WIND Attribute

### Weak Days: D1, D3, D4, D5, D8, D9, D10, D13 (8 days)
- Yes: D3, D4, D5, D9, D10, D13 = 6
- No: D1, D8 = 2

```
Entropy(Weak) = -(6/8 × log₂(6/8)) - (2/8 × log₂(2/8))
              = 0.3112 + 0.5
              = 0.8112 bits
```

### Strong Days: D2, D6, D7, D11, D12, D14 (6 days)
- Yes: D7, D11, D12 = 3
- No: D2, D6, D14 = 3

```
Entropy(Strong) = -(0.5 × log₂(0.5)) - (0.5 × log₂(0.5))
                = 1.0 bits
```

### Weighted Average Entropy

```
Entropy(Wind) = (8/14 × 0.8112) + (6/14 × 1.0)
              = 0.4635 + 0.4286
              = 0.8921 bits
```

### Information Gain (Wind)

```
IG(Wind) = 0.9387 - 0.8921 = 0.0466 bits
```

---

## Information Gain Comparison (Level 1)

| Attribute | Information Gain | Ranking |
|-----------|------------------|---------|
| **Outlook** | **0.2451** | 🥇 BEST |
| Humidity | 0.1516 | 🥈 2nd |
| Wind | 0.0466 | 🥉 3rd |
| Temperature | 0.0277 | 4th |

**🎯 Decision: ROOT NODE = OUTLOOK**

---

## Building the Decision Tree

### Branch 1: OUTLOOK = OVERCAST

**Overcast Days:** D3, D7, D12, D13
- All are PlayTennis = Yes
- Entropy = 0 (PURE)

**✅ LEAF NODE: PlayTennis = YES**

---

### Branch 2: OUTLOOK = SUNNY

**Sunny Days:** D1, D2, D8, D9, D11 (5 days)
- Yes: 2 (D9, D11)
- No: 3 (D1, D2, D8)
- Entropy = 0.9710 bits (MIXED - Need to split)

#### Finding Best Split for Sunny Branch

**Testing HUMIDITY on Sunny Days:**

**High (Sunny):** D1, D2, D8
- All are PlayTennis = No
- Entropy = 0 (PURE)

**Normal (Sunny):** D9, D11
- All are PlayTennis = Yes
- Entropy = 0 (PURE)

```
Entropy(Sunny-Humidity) = (3/5 × 0) + (2/5 × 0) = 0 bits
IG(Sunny-Humidity) = 0.9710 - 0 = 0.9710 bits ⭐ BEST
```

**✅ Split Sunny on HUMIDITY:**
- Sunny + High → PlayTennis = **No**
- Sunny + Normal → PlayTennis = **Yes**

---

### Branch 3: OUTLOOK = RAIN

**Rain Days:** D4, D5, D6, D10, D14 (5 days)
- Yes: 3 (D4, D5, D10)
- No: 2 (D6, D14)
- Entropy = 0.9710 bits (MIXED - Need to split)

#### Finding Best Split for Rain Branch

**Testing WIND on Rain Days:**

**Weak (Rain):** D4, D5, D10
- All are PlayTennis = Yes
- Entropy = 0 (PURE)

**Strong (Rain):** D6, D14
- All are PlayTennis = No
- Entropy = 0 (PURE)

```
Entropy(Rain-Wind) = (3/5 × 0) + (2/5 × 0) = 0 bits
IG(Rain-Wind) = 0.9710 - 0 = 0.9710 bits ⭐ BEST
```

**✅ Split Rain on WIND:**
- Rain + Weak → PlayTennis = **Yes**
- Rain + Strong → PlayTennis = **No**

---

## Final Tree and Decision Rules

### Decision Tree Structure

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

| Day | Outlook | Temp | Humidity | Wind | Prediction | Actual | ✓ |
|-----|---------|------|----------|------|-----------|--------|---|
| D1 | Sunny | Hot | High | Weak | **No** | No | ✓ |
| D2 | Sunny | Hot | High | Strong | **No** | No | ✓ |
| D3 | Overcast | Hot | High | Weak | **Yes** | Yes | ✓ |
| D4 | Rain | Mild | High | Weak | **Yes** | Yes | ✓ |
| D5 | Rain | Cool | Normal | Weak | **Yes** | Yes | ✓ |
| D6 | Rain | Cool | Normal | Strong | **No** | No | ✓ |
| D7 | Overcast | Cool | Normal | Strong | **Yes** | Yes | ✓ |
| D8 | Sunny | Mild | High | Weak | **No** | No | ✓ |
| D9 | Sunny | Cool | Normal | Weak | **Yes** | Yes | ✓ |
| D10 | Rain | Mild | Normal | Weak | **Yes** | Yes | ✓ |
| D11 | Sunny | Mild | Normal | Strong | **Yes** | Yes | ✓ |
| D12 | Overcast | Mild | High | Strong | **Yes** | Yes | ✓ |
| D13 | Overcast | Hot | Normal | Weak | **Yes** | Yes | ✓ |
| D14 | Rain | Mild | High | Strong | **No** | No | ✓ |

**🎯 Accuracy: 14/14 = 100%** ✨

---

## Key Concepts Summary

### Entropy
- Measures disorder in data
- Range: 0 to 1
- 0 = Pure (all same class)
- 1 = Maximum disorder (equal distribution)

### Information Gain
- Measures improvement from splitting
- IG = Entropy(Before) - Entropy(After)
- Higher IG = Better split
- Used to select best attribute at each node

### Decision Tree Building Process
1. Calculate entropy of entire dataset
2. Calculate information gain for each attribute
3. Choose attribute with highest IG
4. Split dataset based on that attribute
5. Repeat for each subset until pure or stopping criteria

### Tree Characteristics
- **Depth**: 2 levels (+ root)
- **Leaf Nodes**: 5
- **Decision Rules**: 5
- **Accuracy**: 100% on training data

---

## Important Notes

1. **Outlook** was the best first split because it reduced entropy the most (IG = 0.2451)
2. **Overcast** subset was already pure (all Yes), so no further split needed
3. **Sunny** subset was split on **Humidity** (IG = 0.9710)
4. **Rain** subset was split on **Wind** (IG = 0.9710)
5. The final tree perfectly classifies all 14 training examples

---

**Generated for ML Course - Decision Tree Analysis**
*Date: June 15, 2026*
