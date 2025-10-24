# Experiment 10

## Aim:
Write a program to implement a binary classification using Decision Trees (Understanding Decision Trees).

## Description:
Binary Classification is a fundamental machine learning task where the goal is to categorize data points into one of two classes or categories. Decision Trees are a popular supervised learning algorithm that can be used for both classification and regression tasks.

### Decision Trees:
A Decision Tree is a tree-like model where:
- **Internal nodes** represent features (attributes)
- **Branches** represent decision rules
- **Leaf nodes** represent outcomes (class labels)

The tree splits the data based on feature values to maximize information gain or minimize impurity (using criteria like Gini impurity or Entropy).

**Key Concepts:**
- **Gini Impurity**: Measures the probability of incorrectly classifying a randomly chosen element
- **Entropy**: Measures the amount of uncertainty or disorder in the data
- **Information Gain**: The reduction in entropy after splitting on a feature
- **Max Depth**: Maximum depth of the tree to prevent overfitting

**Advantages of Decision Trees:**
- Easy to understand and interpret
- Can handle both numerical and categorical data
- Requires little data preprocessing
- Visual representation helps in understanding decision-making process

## Problem Setup:

**Dataset Components**:
- **Samples**: 100 data points
- **Features**: 4 numeric features
- **Classes**: Binary classification (0 and 1)
- **Training Set**: 70% of data
- **Test Set**: 30% of data

**Decision Tree Parameters**:
- **Criterion**: 'gini' (Gini impurity)
- **Max Depth**: 3 (to prevent overfitting)
- **Random State**: 42 (for reproducibility)

**Evaluation Metrics**:
- **Accuracy**: Percentage of correct predictions
- **Confusion Matrix**: Shows True Positives, True Negatives, False Positives, and False Negatives

## Programme:

```python
# Import necessary libraries
from sklearn.datasets import make_classification
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier, export_text
from sklearn.metrics import accuracy_score, confusion_matrix

# Step 1: Create a sample binary classification dataset
X, y = make_classification(
    n_samples=100,      # Total samples
    n_features=4,       # Number of features
    n_informative=2,    # Features contributing to class
    n_redundant=0,      # Redundant features
    n_classes=2,        # Binary classification
    random_state=42
)

# Step 2: Split the dataset into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=42
)

# Step 3: Create and train the Decision Tree classifier
clf = DecisionTreeClassifier(criterion='gini', max_depth=3, random_state=42)
clf.fit(X_train, y_train)

# Step 4: Make predictions on the test set
y_pred = clf.predict(X_test)

# Step 5: Evaluate the model
accuracy = accuracy_score(y_test, y_pred)
cm = confusion_matrix(y_test, y_pred)

print("Decision Tree Structure:\n")
tree_rules = export_text(clf, feature_names=['Feature 1', 'Feature 2', 'Feature 3', 'Feature 4'])
print(tree_rules)

print(f"\nAccuracy on test set: {accuracy:.2f}")
print("\nConfusion Matrix:")
print(cm)
```

## Output:
```
Decision Tree Structure:

|--- Feature 3 <= 0.20
|   |--- Feature 3 <= -0.24
|   |   |--- class: 0
|   |--- Feature 3 > -0.24
|   |   |--- Feature 4 <= -0.01
|   |   |   |--- class: 1
|   |   |--- Feature 4 > -0.01
|   |   |   |--- class: 0
|--- Feature 3 > 0.20
|   |--- Feature 4 <= 2.35
|   |   |--- class: 1
|   |--- Feature 4 > 2.35
|   |   |--- class: 0

Accuracy on test set: 0.97

Confusion Matrix:
[[20  0]
 [ 1  9]]
```

**Confusion Matrix Breakdown:**
- True Positive (TP): Actual 1 predicted as 1 → 9
- True Negative (TN): Actual 0 predicted as 0 → 20
- False Positive (FP): Actual 0 predicted as 1 → 0
- False Negative (FN): Actual 1 predicted as 0 → 1

## Explanation of the output:

**Decision Tree Structure**: The output displays the tree structure in text format, showing the hierarchical decision-making process:
- The tree starts by checking if Feature 3 is less than or equal to 0.20
- Based on this condition, it branches into two paths
- Each path further splits based on additional feature conditions
- The leaf nodes (indicated by "class:") show the final classification (0 or 1)
- Maximum depth of 3 levels prevents overfitting

**Tree Interpretation**:
- First split: Feature 3 <= 0.20 divides the dataset
- Left branch: Further splits on Feature 3 and Feature 4
- Right branch: Splits primarily on Feature 4
- The tree makes decisions by following the path from root to leaf based on feature values

**Accuracy**: The model achieves 97% accuracy on the test set, meaning it correctly classified 29 out of 30 test samples. This high accuracy indicates that the decision tree has learned meaningful patterns from the training data.

**Confusion Matrix Analysis**:
- **True Negatives (20)**: 20 samples of class 0 were correctly predicted as class 0
- **False Positives (0)**: No class 0 samples were incorrectly predicted as class 1
- **False Negatives (1)**: 1 sample of class 1 was incorrectly predicted as class 0
- **True Positives (9)**: 9 samples of class 1 were correctly predicted as class 1

The confusion matrix shows that the model performs very well with only one misclassification (one false negative). The model is particularly strong at identifying class 0 with perfect precision.

## Explanation of the Programme and Output:

### Dataset Generation:
The `make_classification` function from scikit-learn creates a synthetic binary classification dataset with 100 samples and 4 features. Using `n_informative=2` means only 2 out of 4 features actually contribute to determining the class, making the problem realistic as real-world data often contains irrelevant features.

### Train-Test Split:
The dataset is divided into 70% training data (70 samples) and 30% test data (30 samples). This split allows the model to learn from the training set while evaluating its performance on unseen test data, providing an honest assessment of generalization capability.

### Decision Tree Classifier:
The `DecisionTreeClassifier` is configured with:
- **criterion='gini'**: Uses Gini impurity to measure the quality of splits
- **max_depth=3**: Limits tree depth to 3 levels to prevent overfitting
- **random_state=42**: Ensures reproducibility of results

### Gini Impurity:
Gini impurity measures how often a randomly chosen element would be incorrectly labeled if randomly labeled according to the distribution of labels in the subset. The decision tree algorithm selects splits that minimize Gini impurity, creating purer child nodes.

### Tree Visualization:
The `export_text` function converts the trained decision tree into a human-readable text format, showing:
- The feature and threshold used at each split
- The hierarchical structure of decisions
- The final class predictions at leaf nodes

This visualization helps understand how the model makes decisions and which features are most important.

### Model Evaluation:
- **Accuracy Score**: Calculates the ratio of correct predictions to total predictions
- **Confusion Matrix**: Provides detailed breakdown of prediction performance across both classes

### Advantages Demonstrated:
1. **Interpretability**: The tree structure is easy to understand and explain
2. **No Feature Scaling Required**: Decision trees work directly with feature values
3. **Handles Non-linear Relationships**: Can capture complex decision boundaries
4. **Feature Importance**: The tree reveals which features are most discriminative

### Applications in AI:
Decision Trees are widely used in:
- Medical diagnosis systems (disease classification)
- Credit risk assessment (loan approval/denial)
- Customer segmentation (customer behavior classification)
- Fraud detection (fraudulent/legitimate transactions)
- Quality control (defective/non-defective products)

The simplicity and interpretability of Decision Trees make them particularly valuable in domains where understanding the reasoning behind predictions is crucial, such as healthcare and finance.
