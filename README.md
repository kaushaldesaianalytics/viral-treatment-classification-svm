# Viral Treatment Outcome Classification
## Support Vector Machine Classification

Which combination of two experimental treatments predicts successful virus suppression in mice? This project applies Support Vector Machine classification to a mouse viral study dataset, using the clean two-dimensional feature space to systematically explore how kernel choice, regularization strength, and gamma shape the SVM decision boundary.

---

## Overview

Support Vector Machines find the hyperplane that maximizes the margin between classes. In two dimensions this is a line; in higher dimensions it becomes a plane or hyperplane. The kernel trick allows SVMs to implicitly map features into higher-dimensional spaces, enabling the model to learn non-linear boundaries without computing the transformation explicitly.

This project demonstrates the full SVM workflow: visualizing the feature space, fitting a linear baseline, exploring the effect of each key hyperparameter, and using GridSearchCV to identify the optimal configuration.

---

## Dataset

The mouse viral study dataset records dosage levels of two experimental treatments administered to infected mice, along with a binary outcome indicating whether the virus was detected post-treatment.

| Feature | Description |
|---|---|
| Med_1_mL | Dosage of first treatment in milliliters |
| Med_2_mL | Dosage of second treatment in milliliters |
| Virus Present | Binary target: 1 = virus detected, 0 = not detected |

---

## Workflow

**1. Exploratory Data Analysis**
A scatter plot of both dosage levels colored by viral outcome reveals whether the classes are linearly separable. Clear separation in this dataset confirms SVM is well-suited and allows direct visualization of how different kernels draw their boundaries.

**2. Linear Kernel Baseline**
An SVM with a linear kernel and high C value (C=1000) fits a tight margin between classes. This serves as the interpretable baseline against which non-linear kernels are compared.

**3. Hyperparameter Exploration**

*Regularization (C):*
C controls the penalty for misclassification. A small C (e.g., 0.05) produces a wide, smoother margin that tolerates some misclassifications. A large C enforces a tight margin that fits the training data more precisely but risks overfitting.

*Kernel Type:*
Four kernels are compared: linear, RBF, sigmoid, and polynomial. RBF is the most commonly used non-linear kernel and maps features into an infinite-dimensional space. Polynomial kernels fit curved boundaries of a specified degree. Sigmoid mimics a neural network's activation pattern.

*Gamma:*
For RBF, polynomial, and sigmoid kernels, gamma controls the reach of a single training point's influence. Low gamma produces smooth, broad boundaries; high gamma creates tight, localized boundaries that can overfit.

**4. GridSearchCV**
A 5-fold cross-validated grid search across C values [0.01, 0.1, 1] and kernel types [linear, rbf] identifies the optimal parameter combination. Given the clear class separation in this dataset, both configurations achieve strong accuracy. The primary purpose here is demonstrating the search workflow for more complex problems.

---

## Results

| Configuration | Notes |
|---|---|
| Linear, C=1000 | Tight margin, fully separating |
| Linear, C=0.05 | Wide margin, slightly more permissive |
| RBF, C=1 | Curved boundary following class clusters |
| Poly (degree=2) | Smooth curved boundary |
| Best (GridSearchCV) | 100% accuracy on this separable dataset |

---

## Key Concepts

**Maximal Margin Classifier:** SVMs maximize the distance between the decision boundary and the nearest training points (support vectors). A wider margin generally improves generalization.

**Kernel Trick:** Rather than explicitly computing features in a high-dimensional space, kernels compute dot products in that space implicitly. This makes non-linear classification computationally feasible.

**C as a Bias-Variance Control:** Low C introduces bias by allowing misclassifications in exchange for a wider, more generalizable margin. High C reduces bias but increases variance by fitting the training data more tightly.

**Gamma and Overfitting:** High gamma values make each training point's influence very local, which can produce a highly irregular boundary that memorizes training noise. Low gamma produces smoother, more generalizable boundaries.

---

## Stack

- Python 3
- Pandas, NumPy
- Matplotlib, Seaborn
- scikit-learn (SVC, GridSearchCV)
- `svm_boundary_plotter.py` is a helper script for decision boundary visualization

---

## File Structure

```
viral-treatment-classification-svm/
├── svm_viral_classification.ipynb   # Main project notebook
├── mouse_viral_study.csv            # Dataset
├── svm_boundary_plotter.py          # Decision boundary visualization helper
└── README.md
```

---

## How to Run

1. Clone the repository
2. Install dependencies: `pip install pandas numpy matplotlib seaborn scikit-learn`
3. Ensure `svm_boundary_plotter.py` is in the same directory as the notebook
4. Open `svm_viral_classification.ipynb` in Jupyter and run all cells
