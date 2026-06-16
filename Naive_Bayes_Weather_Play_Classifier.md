# Naive Bayes Classifier: Weather-Play Dataset

## Complete Step-by-Step Guide

---

## Table of Contents
1. [Dataset Overview](#dataset-overview)
2. [Bayes' Theorem Explanation](#bayes-theorem-explanation)
3. [Prior Probabilities](#prior-probabilities)
4. [Likelihood Calculations](#likelihood-calculations)
5. [Posterior Probability Calculations](#posterior-probability-calculations)
6. [Making Predictions](#making-predictions)
7. [Verification](#verification)

---

## Dataset Overview

The Weather-Play dataset contains information about 14 days with weather conditions and whether people played (Yes/No).

### Dataset Table

| Day | Weather | Play |
|-----|---------|------|
| 1 | Sunny | No |
| 2 | Sunny | No |
| 3 | Overcast | Yes |
| 4 | Rainy | Yes |
| 5 | Rainy | Yes |
| 6 | Rainy | No |
| 7 | Overcast | Yes |
| 8 | Sunny | No |
| 9 | Sunny | Yes |
| 10 | Rainy | Yes |
| 11 | Sunny | Yes |
| 12 | Overcast | Yes |
| 13 | Overcast | Yes |
| 14 | Rainy | No |

### Data Summary
- **Total Days**: 14
- **Target Variable**: Play (Yes/No)
- **Feature**: Weather (Sunny, Overcast, Rainy)
- **Play Distribution**:
  - Yes: 9 days
  - No: 5 days

---

## Bayes' Theorem Explanation

### What is Naive Bayes?

Naive Bayes is a probabilistic classifier based on **Bayes' Theorem**. It calculates the probability of a class given the features.

### Formula

```
P(Class|Features) = (P(Features|Class) × P(Class)) / P(Features)

Where:
- P(Class|Features) = Posterior Probability (what we want to find)
- P(Features|Class) = Likelihood (probability of features given class)
- P(Class) = Prior Probability (probability of class)
- P(Features) = Evidence (overall probability of features)
```

### Simplified Naive Bayes Formula

Since we only have one feature (Weather), the formula becomes:

```
P(Class|Weather) = (P(Weather|Class) × P(Class)) / P(Weather)
```

### Why "Naive"?

The classifier assumes that **all features are independent** of each other. This is a simplifying assumption that is often not true in reality, but it works well in practice and makes calculations simpler.

---

## Prior Probabilities

### Prior Probability: P(Class)

Prior probability is the probability of each class **before** considering any features.

### Count Instances

**From the dataset:**
- Play = Yes: Days 3, 4, 5, 7, 10, 12, 13 = **9 days**
- Play = No: Days 1, 2, 6, 8, 14 = **5 days**
- Total: 14 days

### Calculate Prior Probabilities

```
P(Play = Yes) = 9/14 = 0.6429

P(Play = No) = 5/14 = 0.3571
```

**Prior Probabilities Summary:**

| Class | Count | Probability |
|-------|-------|------------|
| Play = Yes | 9 | 0.6429 |
| Play = No | 5 | 0.3571 |
| Total | 14 | 1.0000 |

---

## Likelihood Calculations

### Likelihood: P(Weather|Class)

Likelihood is the probability of observing a feature **given** a class.

### Weather Distribution by Class

#### Weather when Play = Yes

Days with Play = Yes: 3, 4, 5, 7, 10, 12, 13

| Weather | Days | Count |
|---------|------|-------|
| Sunny | 9, 11 | 2 |
| Overcast | 3, 7, 12, 13 | 4 |
| Rainy | 4, 5, 10 | 3 |
| **Total** | - | **9** |

**Likelihoods for Play = Yes:**

```
P(Sunny|Play=Yes) = 2/9 = 0.2222
P(Overcast|Play=Yes) = 4/9 = 0.4444
P(Rainy|Play=Yes) = 3/9 = 0.3333
```

---

#### Weather when Play = No

Days with Play = No: 1, 2, 6, 8, 14

| Weather | Days | Count |
|---------|------|-------|
| Sunny | 1, 2, 8 | 3 |
| Overcast | - | 0 |
| Rainy | 6, 14 | 2 |
| **Total** | - | **5** |

**Likelihoods for Play = No:**

```
P(Sunny|Play=No) = 3/5 = 0.6

P(Overcast|Play=No) = 0/5 = 0 ⚠️ (Problem: Zero probability!)

P(Rainy|Play=No) = 2/5 = 0.4
```

---

### Handling Zero Probability (Laplace Smoothing)

When a feature-class combination has zero occurrences, we use **Laplace Smoothing** to avoid multiplying by zero.

**Formula:**
```
P(Feature|Class) = (Count + 1) / (Total + Number of Features)
```

**Applying Laplace Smoothing:**

For Play = No:
```
P(Sunny|Play=No) = (3 + 1) / (5 + 3) = 4/8 = 0.5

P(Overcast|Play=No) = (0 + 1) / (5 + 3) = 1/8 = 0.125

P(Rainy|Play=No) = (2 + 1) / (5 + 3) = 3/8 = 0.375
```

For Play = Yes:
```
P(Sunny|Play=Yes) = (2 + 1) / (9 + 3) = 3/12 = 0.25

P(Overcast|Play=Yes) = (4 + 1) / (9 + 3) = 5/12 = 0.4167

P(Rainy|Play=Yes) = (3 + 1) / (9 + 3) = 4/12 = 0.3333
```

---

### Likelihood Summary Table

| Weather | P(Weather\|Play=Yes) | P(Weather\|Play=No) |
|---------|----------------------|----------------------|
| Sunny | 0.25 | 0.5 |
| Overcast | 0.4167 | 0.125 |
| Rainy | 0.3333 | 0.375 |

---

## Posterior Probability Calculations

### Posterior: P(Class|Weather)

Using Bayes' Theorem to find the probability of each class given a weather condition.

### Formula (Simplified)

Since P(Weather) is the same for both classes, we can ignore it and use:

```
P(Class|Weather) ∝ P(Weather|Class) × P(Class)

(The ∝ symbol means "proportional to")
```

To get actual probabilities, we normalize:

```
P(Class|Weather) = (P(Weather|Class) × P(Class)) / Sum of all class probabilities
```

---

### Case 1: Weather = Sunny

#### Calculate unnormalized probabilities

```
P(Play=Yes|Sunny) ∝ P(Sunny|Play=Yes) × P(Play=Yes)
                  = 0.25 × 0.6429
                  = 0.1607

P(Play=No|Sunny) ∝ P(Sunny|Play=No) × P(Play=No)
                 = 0.5 × 0.3571
                 = 0.1786
```

#### Normalize

```
Sum = 0.1607 + 0.1786 = 0.3393

P(Play=Yes|Sunny) = 0.1607 / 0.3393 = 0.4736 (47.36%)

P(Play=No|Sunny) = 0.1786 / 0.3393 = 0.5264 (52.64%)
```

**Prediction for Sunny: Play = NO** (higher probability)

---

### Case 2: Weather = Overcast

#### Calculate unnormalized probabilities

```
P(Play=Yes|Overcast) ∝ P(Overcast|Play=Yes) × P(Play=Yes)
                     = 0.4167 × 0.6429
                     = 0.2679

P(Play=No|Overcast) ∝ P(Overcast|Play=No) × P(Play=No)
                    = 0.125 × 0.3571
                    = 0.0446
```

#### Normalize

```
Sum = 0.2679 + 0.0446 = 0.3125

P(Play=Yes|Overcast) = 0.2679 / 0.3125 = 0.8573 (85.73%)

P(Play=No|Overcast) = 0.0446 / 0.3125 = 0.1427 (14.27%)
```

**Prediction for Overcast: Play = YES** (higher probability)

---

### Case 3: Weather = Rainy

#### Calculate unnormalized probabilities

```
P(Play=Yes|Rainy) ∝ P(Rainy|Play=Yes) × P(Play=Yes)
                  = 0.3333 × 0.6429
                  = 0.2143

P(Play=No|Rainy) ∝ P(Rainy|Play=No) × P(Play=No)
                 = 0.375 × 0.3571
                 = 0.1339
```

#### Normalize

```
Sum = 0.2143 + 0.1339 = 0.3482

P(Play=Yes|Rainy) = 0.2143 / 0.3482 = 0.6154 (61.54%)

P(Play=No|Rainy) = 0.1339 / 0.3482 = 0.3846 (38.46%)
```

**Prediction for Rainy: Play = YES** (higher probability)

---

### Posterior Probability Summary Table

| Weather | P(Yes\|Weather) | P(No\|Weather) | Prediction |
|---------|-----------------|----------------|-----------|
| Sunny | 47.36% | 52.64% | **No** |
| Overcast | 85.73% | 14.27% | **Yes** |
| Rainy | 61.54% | 38.46% | **Yes** |

---

## Making Predictions

### Decision Rule

For a new day with a given weather condition, predict the class with **higher posterior probability**.

```
If P(Play=Yes|Weather) > P(Play=No|Weather) → Predict: Yes
Otherwise → Predict: No
```

### Prediction Examples

**Example 1: New day with Sunny weather**
```
P(Yes|Sunny) = 47.36%
P(No|Sunny) = 52.64%

Prediction: NO (52.64% > 47.36%)
Confidence: 52.64%
```

**Example 2: New day with Overcast weather**
```
P(Yes|Overcast) = 85.73%
P(No|Overcast) = 14.27%

Prediction: YES (85.73% > 14.27%)
Confidence: 85.73%
```

**Example 3: New day with Rainy weather**
```
P(Yes|Rainy) = 61.54%
P(No|Rainy) = 38.46%

Prediction: YES (61.54% > 38.46%)
Confidence: 61.54%
```

---

## Verification

### Testing Naive Bayes on Training Data

| Day | Weather | Prediction | Actual | ✓/✗ |
|-----|---------|-----------|--------|-----|
| 1 | Sunny | No | No | ✓ |
| 2 | Sunny | No | No | ✓ |
| 3 | Overcast | Yes | Yes | ✓ |
| 4 | Rainy | Yes | Yes | ✓ |
| 5 | Rainy | Yes | Yes | ✓ |
| 6 | Rainy | Yes | No | ✗ |
| 7 | Overcast | Yes | Yes | ✓ |
| 8 | Sunny | No | No | ✓ |
| 9 | Sunny | No | Yes | ✗ |
| 10 | Rainy | Yes | Yes | ✓ |
| 11 | Sunny | No | Yes | ✗ |
| 12 | Overcast | Yes | Yes | ✓ |
| 13 | Overcast | Yes | Yes | ✓ |
| 14 | Rainy | Yes | No | ✗ |

---

### Results

**Correct Predictions**: 10 days (Days 1, 2, 3, 4, 5, 7, 8, 10, 12, 13)

**Incorrect Predictions**: 4 days (Days 6, 9, 11, 14)

```
Accuracy = 10 / 14 = 0.7143 = 71.43%
```

### Confusion Matrix

|  | Predicted: Yes | Predicted: No | Total |
|---|---|---|---|
| **Actual: Yes** | 6 (True Positive) | 3 (False Negative) | 9 |
| **Actual: No** | 1 (False Positive) | 4 (True Negative) | 5 |
| **Total** | 7 | 7 | 14 |

---

### Performance Metrics

```
True Positive Rate (Sensitivity) = TP / (TP + FN)
                                 = 6 / 9
                                 = 0.6667 = 66.67%

True Negative Rate (Specificity) = TN / (TN + FP)
                                 = 4 / 5
                                 = 0.8 = 80%

Precision = TP / (TP + FP)
          = 6 / 7
          = 0.8571 = 85.71%

F1-Score = 2 × (Precision × Recall) / (Precision + Recall)
         = 2 × (0.8571 × 0.6667) / (0.8571 + 0.6667)
         = 0.7500 = 75%
```

---

## Detailed Error Analysis

### Misclassified Instances

**Day 6: Rainy → Predicted: Yes, Actual: No**
- P(Yes|Rainy) = 61.54% > P(No|Rainy) = 38.46%
- Model confident it should play, but actual = No
- Close probabilities, minor error

**Day 9: Sunny → Predicted: No, Actual: Yes**
- P(Yes|Sunny) = 47.36% < P(No|Sunny) = 52.64%
- Model thought it shouldn't play, but actual = Yes
- Close probabilities, borderline case

**Day 11: Sunny → Predicted: No, Actual: Yes**
- P(Yes|Sunny) = 47.36% < P(No|Sunny) = 52.64%
- Same as Day 9
- Two sunny days resulted in playing

**Day 14: Rainy → Predicted: Yes, Actual: No**
- P(Yes|Rainy) = 61.54% > P(No|Rainy) = 38.46%
- Model confident it should play, but actual = No
- Same issue as Day 6

---

## Key Insights

### Why Some Errors Occur

1. **Sunny Days (Days 9, 11)**: While most sunny days don't have people playing, some do. Naive Bayes captures the majority trend but misses minority cases.

2. **Rainy Days (Days 6, 14)**: While most rainy days have people playing, some don't. Again, majority trend vs minority cases.

3. **Overcast Days**: Perfect predictions! All overcast days show consistent behavior (always Yes).

### Strengths of Naive Bayes

✅ Simple and fast  
✅ Works well with small datasets  
✅ Provides probability estimates  
✅ Easy to interpret  
✅ Handles missing data well  

### Limitations of Naive Bayes

❌ Assumes feature independence (may not always be true)  
❌ Sensitive to zero probabilities (needs smoothing)  
❌ Can be outperformed by more complex models  
❌ Assumes all features are equally important  

---

## Classification Rules Summary

### Learned Model Rules

1. **If Weather = Overcast** → **Predict: YES** (85.73% confidence)
2. **If Weather = Rainy** → **Predict: YES** (61.54% confidence)
3. **If Weather = Sunny** → **Predict: NO** (52.64% confidence)

---

## Probability Reference Table

### All Conditional Probabilities

| Weather | P(W\|Yes) | P(W\|No) | P(Yes\|W) | P(No\|W) | Prediction |
|---------|-----------|----------|-----------|----------|-----------|
| Sunny | 0.25 | 0.5 | 0.4736 | 0.5264 | **No** |
| Overcast | 0.4167 | 0.125 | 0.8573 | 0.1427 | **Yes** |
| Rainy | 0.3333 | 0.375 | 0.6154 | 0.3846 | **Yes** |

---

## Implementation Summary

### Naive Bayes Classifier Model

| Aspect | Value |
|--------|-------|
| Algorithm | Naive Bayes (Multinomial) |
| Feature | Weather (Categorical) |
| Target | Play (Binary: Yes/No) |
| Training Samples | 14 |
| Training Accuracy | 71.43% (10/14) |
| Smoothing Method | Laplace Smoothing |
| Classes | 2 |
| Features | 1 |
| Feature Values | 3 (Sunny, Overcast, Rainy) |

---

## Formulas Reference

### Bayes' Theorem
```
P(A|B) = (P(B|A) × P(A)) / P(B)
```

### Laplace Smoothing
```
P(Feature|Class) = (Count + 1) / (Total + |Features|)
```

### Posterior Probability
```
P(Class|Features) = (P(Features|Class) × P(Class)) / P(Features)
```

### Accuracy
```
Accuracy = (TP + TN) / (TP + TN + FP + FN)
```

### Precision
```
Precision = TP / (TP + FP)
```

### Recall (Sensitivity)
```
Recall = TP / (TP + FN)
```

### F1-Score
```
F1 = 2 × (Precision × Recall) / (Precision + Recall)
```

---

## Conclusion

The Naive Bayes Classifier successfully learned the relationship between Weather and Play behavior:
- **Overcast weather strongly indicates playing** (85.73%)
- **Rainy weather moderately indicates playing** (61.54%)
- **Sunny weather slightly indicates not playing** (52.64%)

Despite achieving 71.43% accuracy on training data, there's room for improvement. This could be achieved by:
1. Collecting more data
2. Adding more features (e.g., Temperature, Humidity)
3. Using more sophisticated models
4. Adjusting decision thresholds

---

**Generated for ML Course - Naive Bayes Classifier Analysis**
*Dataset: Weather-Play (14 Samples, Binary Classification)*
*Date: June 15, 2026*
