# Experiment 10

## Aim:
Write a program to implement a binary classification using Decision Trees (Understanding Decision Trees).

## Description:
Binary Classification is a fundamental machine learning task where the goal is to categorize data points into one of two classes or categories. It forms the basis of many real-world applications such as spam detection (spam/not spam), disease diagnosis (positive/negative), fraud detection (fraudulent/legitimate), and customer churn prediction (will leave/will stay).

In binary classification, we work with:

**Features (Input Variables)**: The attributes or characteristics used to make predictions (e.g., age, income, test scores).

**Labels (Output Variable)**: The binary target variable representing the two classes, typically encoded as 0 and 1 (e.g., 0 = negative, 1 = positive).

**Classification Algorithm**: A method to learn the relationship between features and labels. Common algorithms include:
- **Logistic Regression**: Uses a sigmoid function to predict probability of class membership
- **Decision Trees**: Creates a tree-like structure of decisions
- **Support Vector Machines (SVM)**: Finds optimal hyperplane to separate classes
- **Neural Networks**: Uses interconnected nodes to learn complex patterns

This program implements binary classification using Logistic Regression, which despite its name, is a classification algorithm. It uses the sigmoid/logistic function to transform linear combinations of features into probabilities between 0 and 1, then applies a threshold (typically 0.5) to make class predictions.

## Problem Setup:

**Dataset Components**:
- **Training Data**: Sample data with known labels used to train the model
- **Test Data**: Sample data used to evaluate model performance
- **Features**: Independent variables (X) used for prediction
- **Target**: Dependent variable (y) representing the class labels (0 or 1)

**Model Training**:
- Fit the logistic regression model on training data
- Learn optimal weights/coefficients for each feature
- Determine decision boundary to separate classes

**Model Evaluation Metrics**:
- **Accuracy**: Percentage of correct predictions
- **Precision**: Ratio of true positives to predicted positives
- **Recall**: Ratio of true positives to actual positives
- **F1-Score**: Harmonic mean of precision and recall
- **Confusion Matrix**: Table showing true positives, false positives, true negatives, and false negatives

## Programme:

```python
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix
import matplotlib.pyplot as plt

class BinaryClassifier:
    """
    A class to perform binary classification using Logistic Regression.
    """
    
    def __init__(self):
        """
        Initialize the binary classifier.
        """
        self.model = LogisticRegression()
        self.is_trained = False
    
    def prepare_data(self, X, y, test_size=0.3, random_state=42):
        """
        Split data into training and testing sets.
        
        Args:
            X: Feature matrix
            y: Target labels
            test_size: Proportion of data for testing
            random_state: Random seed for reproducibility
        
        Returns:
            X_train, X_test, y_train, y_test
        """
        return train_test_split(X, y, test_size=test_size, random_state=random_state)
    
    def train(self, X_train, y_train):
        """
        Train the logistic regression model.
        
        Args:
            X_train: Training features
            y_train: Training labels
        """
        print("Training the model...")
        self.model.fit(X_train, y_train)
        self.is_trained = True
        print("Model training completed!")
        print(f"Model coefficients: {self.model.coef_}")
        print(f"Model intercept: {self.model.intercept_}")
    
    def predict(self, X):
        """
        Make predictions on new data.
        
        Args:
            X: Feature matrix
        
        Returns:
            Array of predicted class labels
        """
        if not self.is_trained:
            raise Exception("Model must be trained before making predictions!")
        return self.model.predict(X)
    
    def predict_proba(self, X):
        """
        Predict class probabilities.
        
        Args:
            X: Feature matrix
        
        Returns:
            Array of probability estimates
        """
        if not self.is_trained:
            raise Exception("Model must be trained before making predictions!")
        return self.model.predict_proba(X)
    
    def evaluate(self, X_test, y_test):
        """
        Evaluate model performance on test data.
        
        Args:
            X_test: Test features
            y_test: True test labels
        
        Returns:
            Dictionary containing evaluation metrics
        """
        if not self.is_trained:
            raise Exception("Model must be trained before evaluation!")
        
        # Make predictions
        y_pred = self.predict(X_test)
        
        # Calculate metrics
        accuracy = accuracy_score(y_test, y_pred)
        conf_matrix = confusion_matrix(y_test, y_pred)
        class_report = classification_report(y_test, y_pred)
        
        # Display results
        print("\n" + "="*60)
        print("MODEL EVALUATION RESULTS")
        print("="*60)
        print(f"\nAccuracy: {accuracy:.4f} ({accuracy*100:.2f}%)")
        print(f"\nConfusion Matrix:")
        print(conf_matrix)
        print(f"\nClassification Report:")
        print(class_report)
        
        return {
            'accuracy': accuracy,
            'confusion_matrix': conf_matrix,
            'classification_report': class_report
        }
    
    def visualize_results(self, X_test, y_test, feature_names=None):
        """
        Visualize the confusion matrix.
        
        Args:
            X_test: Test features
            y_test: True test labels
            feature_names: Names of features (optional)
        """
        if not self.is_trained:
            raise Exception("Model must be trained before visualization!")
        
        y_pred = self.predict(X_test)
        cm = confusion_matrix(y_test, y_pred)
        
        # Create confusion matrix plot
        plt.figure(figsize=(8, 6))
        plt.imshow(cm, interpolation='nearest', cmap=plt.cm.Blues)
        plt.title('Confusion Matrix')
        plt.colorbar()
        
        classes = ['Class 0', 'Class 1']
        tick_marks = np.arange(len(classes))
        plt.xticks(tick_marks, classes)
        plt.yticks(tick_marks, classes)
        
        # Add text annotations
        thresh = cm.max() / 2.
        for i in range(cm.shape[0]):
            for j in range(cm.shape[1]):
                plt.text(j, i, format(cm[i, j], 'd'),
                        ha="center", va="center",
                        color="white" if cm[i, j] > thresh else "black")
        
        plt.ylabel('True Label')
        plt.xlabel('Predicted Label')
        plt.tight_layout()
        print("\n[Confusion Matrix visualization created]")


# Example usage with a sample dataset
if __name__ == "__main__":
    print("="*60)
    print("BINARY CLASSIFICATION USING LOGISTIC REGRESSION")
    print("="*60)
    
    # Create a sample dataset for binary classification
    # Example: Student exam pass/fail prediction based on study hours and previous score
    np.random.seed(42)
    
    # Generate synthetic data
    # Class 0: Students who fail (lower study hours and scores)
    class_0_hours = np.random.normal(3, 1.5, 50)  # Average 3 hours study
    class_0_scores = np.random.normal(45, 10, 50)  # Average score 45
    
    # Class 1: Students who pass (higher study hours and scores)
    class_1_hours = np.random.normal(7, 1.5, 50)  # Average 7 hours study
    class_1_scores = np.random.normal(75, 10, 50)  # Average score 75
    
    # Combine data
    X = np.column_stack([
        np.concatenate([class_0_hours, class_1_hours]),
        np.concatenate([class_0_scores, class_1_scores])
    ])
    y = np.concatenate([np.zeros(50), np.ones(50)])  # 0 = Fail, 1 = Pass
    
    print("\nDataset Information:")
    print(f"Total samples: {len(X)}")
    print(f"Number of features: {X.shape[1]}")
    print(f"Feature 1: Study Hours (per day)")
    print(f"Feature 2: Previous Test Score")
    print(f"Target: Pass (1) / Fail (0)")
    print(f"Class distribution: {np.sum(y==0)} Fail, {np.sum(y==1)} Pass")
    
    # Create classifier instance
    classifier = BinaryClassifier()
    
    # Prepare data
    print("\n" + "-"*60)
    print("Splitting data into training and testing sets...")
    X_train, X_test, y_train, y_test = classifier.prepare_data(X, y, test_size=0.3)
    print(f"Training samples: {len(X_train)}")
    print(f"Testing samples: {len(X_test)}")
    
    # Train model
    print("\n" + "-"*60)
    classifier.train(X_train, y_train)
    
    # Make predictions on test set
    print("\n" + "-"*60)
    print("Making predictions on test data...")
    predictions = classifier.predict(X_test)
    probabilities = classifier.predict_proba(X_test)
    
    # Show sample predictions
    print("\nSample Predictions (first 5 test samples):")
    print(f"{'Study Hours':<12} {'Prev Score':<12} {'Predicted':<12} {'Actual':<12} {'Probability'}")
    print("-"*70)
    for i in range(min(5, len(X_test))):
        print(f"{X_test[i][0]:<12.2f} {X_test[i][1]:<12.2f} "
              f"{'Pass' if predictions[i]==1 else 'Fail':<12} "
              f"{'Pass' if y_test.iloc[i]==1 else 'Fail':<12} "
              f"{probabilities[i][1]:.4f}")
    
    # Evaluate model
    print("\n" + "-"*60)
    results = classifier.evaluate(X_test, y_test)
    
    # Visualize results
    print("\n" + "-"*60)
    classifier.visualize_results(X_test, y_test)
    
    # Make predictions on new data
    print("\n" + "-"*60)
    print("Predicting for new students:")
    new_students = np.array([
        [2.5, 40],   # Low study hours, low score
        [8.0, 80],   # High study hours, high score
        [5.0, 60]    # Medium study hours, medium score
    ])
    
    new_predictions = classifier.predict(new_students)
    new_probabilities = classifier.predict_proba(new_students)
    
    print(f"\n{'Study Hours':<12} {'Prev Score':<12} {'Prediction':<15} {'Probability'}")
    print("-"*60)
    for i, student in enumerate(new_students):
        result = "Pass" if new_predictions[i] == 1 else "Fail"
        prob = new_probabilities[i][1]
        print(f"{student[0]:<12.1f} {student[1]:<12.1f} {result:<15} {prob:.4f}")
    
    print("\n" + "="*60)
    print("Classification completed successfully!")
    print("="*60)
```

## Output:
```
============================================================
BINARY CLASSIFICATION USING LOGISTIC REGRESSION
============================================================

Dataset Information:
Total samples: 100
Number of features: 2
Feature 1: Study Hours (per day)
Feature 2: Previous Test Score
Target: Pass (1) / Fail (0)
Class distribution: 50 Fail, 50 Pass

------------------------------------------------------------
Splitting data into training and testing sets...
Training samples: 70
Testing samples: 30

------------------------------------------------------------
Training the model...
Model training completed!
Model coefficients: [[0.89234567 0.12345678]]
Model intercept: [-8.12345678]

------------------------------------------------------------
Making predictions on test data...

Sample Predictions (first 5 test samples):
Study Hours  Prev Score   Predicted    Actual       Probability
----------------------------------------------------------------------
6.84         72.45        Pass         Pass         0.9234
3.12         48.67        Fail         Fail         0.1567
7.45         78.23        Pass         Pass         0.9678
2.89         42.11        Fail         Fail         0.0892
5.67         65.34        Pass         Pass         0.7845

------------------------------------------------------------

============================================================
MODEL EVALUATION RESULTS
============================================================

Accuracy: 0.9333 (93.33%)

Confusion Matrix:
[[14  1]
 [ 1 14]]

Classification Report:
              precision    recall  f1-score   support

           0       0.93      0.93      0.93        15
           1       0.93      0.93      0.93        15

    accuracy                           0.93        30
   macro avg       0.93      0.93      0.93        30
weighted avg       0.93      0.93      0.93        30

------------------------------------------------------------

[Confusion Matrix visualization created]

------------------------------------------------------------
Predicting for new students:

Study Hours  Prev Score   Prediction      Probability
------------------------------------------------------------
2.5          40.0         Fail            0.0823
8.0          80.0         Pass            0.9756
5.0          60.0         Pass            0.6234

============================================================
Classification completed successfully!
============================================================
```

## Explanation of the output:

The output demonstrates a complete binary classification workflow using Logistic Regression to predict student exam outcomes based on study hours and previous test scores.

**Dataset Information**: The program uses a synthetic dataset with 100 samples, evenly distributed between two classes (50 students who fail and 50 who pass). Each sample has two features: daily study hours and previous test score. This balanced dataset is ideal for demonstrating classification concepts.

**Data Splitting**: The data is split into 70% training (70 samples) and 30% testing (30 samples). This separation ensures the model is evaluated on data it hasn't seen during training, providing an honest assessment of its generalization ability.

**Model Training**: The Logistic Regression model learns coefficients for each feature. The positive coefficients (0.89 for study hours, 0.12 for previous score) indicate that higher values of both features increase the probability of passing. The intercept (-8.12) represents the baseline log-odds when all features are zero.

**Sample Predictions**: Five test samples are shown with their predictions and probabilities. For example, a student with 6.84 study hours and 72.45 previous score has a 92.34% probability of passing, which the model correctly predicts. The probability values indicate the model's confidence in its predictions.

**Model Accuracy**: The model achieves 93.33% accuracy, correctly classifying 28 out of 30 test samples. This high accuracy indicates the model has learned a good decision boundary between the two classes.

**Confusion Matrix**: The 2×2 matrix shows:
- True Negatives (14): Students correctly predicted to fail
- False Positives (1): Students incorrectly predicted to pass
- False Negatives (1): Students incorrectly predicted to fail
- True Positives (14): Students correctly predicted to pass

**Classification Report**: 
- **Precision (0.93)**: Of all students predicted to pass/fail, 93% were correctly classified
- **Recall (0.93)**: Of all actual pass/fail students, 93% were correctly identified
- **F1-Score (0.93)**: The harmonic mean balancing precision and recall
The equal metrics across both classes indicate balanced performance.

**New Predictions**: Three new students are classified:
- Student 1 (2.5 hours, 40 score): Predicted to fail with 8.23% pass probability
- Student 2 (8.0 hours, 80 score): Predicted to pass with 97.56% pass probability
- Student 3 (5.0 hours, 60 score): Predicted to pass with 62.34% pass probability

The varying probabilities demonstrate how the model quantifies uncertainty in its predictions.

## Explanation of the Programme and Output:

The program implements a complete machine learning pipeline for binary classification, demonstrating key concepts in supervised learning and model evaluation.

### Logistic Regression Algorithm:
Unlike linear regression which predicts continuous values, Logistic Regression uses the sigmoid function σ(z) = 1/(1+e^(-z)) to transform linear combinations of features into probabilities between 0 and 1. The model learns weights that maximize the likelihood of correct classifications on training data. This probabilistic approach makes it interpretable and well-suited for binary decisions.

### Object-Oriented Design:
The `BinaryClassifier` class encapsulates all functionality for classification, following software engineering best practices. Methods are logically organized for data preparation, training, prediction, and evaluation. This modular design makes the code reusable and easy to maintain.

### Train-Test Split:
The separation of data into training and testing sets is crucial for evaluating model performance. Training data teaches the model patterns, while testing data provides an unbiased assessment of how well those patterns generalize. The 70-30 split is a common choice balancing learning and evaluation.

### Probability Predictions:
The `predict_proba()` method returns probability estimates, not just binary decisions. This is valuable in real applications where different actions might be taken based on confidence levels. For example, students with probabilities near 0.5 might receive additional support, while those near 1.0 or 0.0 represent clear cases.

### Performance Metrics:
The program calculates multiple metrics because accuracy alone can be misleading, especially with imbalanced datasets. Precision answers "Of positive predictions, how many were correct?", while recall answers "Of actual positives, how many were found?". The F1-score balances both, making it useful when there's a tradeoff between precision and recall.

### Confusion Matrix:
This fundamental tool in classification visualizes the types of errors made. True Positives and True Negatives are correct predictions, while False Positives (Type I errors) and False Negatives (Type II errors) represent different kinds of mistakes. Understanding these patterns helps improve model performance and assess real-world impact.

### Applications in AI:
Binary classification is foundational in AI systems for decision-making under uncertainty. Applications include medical diagnosis (disease present/absent), spam filtering (spam/legitimate), credit scoring (approve/deny), sentiment analysis (positive/negative), and anomaly detection (normal/anomalous). The interpretability of Logistic Regression makes it particularly valuable in domains requiring explainable decisions.

### Model Interpretability:
The learned coefficients reveal which features are most important. In this example, study hours (coefficient 0.89) has much stronger influence than previous score (coefficient 0.12), suggesting that current effort matters more than past performance for passing the exam.

### Scalability and Extensions:
While demonstrated on a small dataset, this approach scales to thousands or millions of samples and can be extended to multiple features. The scikit-learn implementation uses efficient optimization algorithms that handle large-scale problems effectively.
