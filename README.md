# Optimal Sparse Decision Trees (OSDT)

**Course:** 3011L Machine Learning Laboratory  
**Author:** Sahil Shamrao Rathod  
**Roll No:** 240150048  

---

## Project Overview

This repository contains a Python implementation of the NeurIPS 2019 research paper **"Optimal Sparse Decision Trees"** by Xiyang Hu, Cynthia Rudin, and Margo Seltzer. 

Traditional decision tree algorithms (like CART and C4.5) rely on greedy, top-down splitting heuristics that can easily get trapped in local optima, producing suboptimal trees. This project implements a branch-and-bound optimization framework that searches the space of binary decision trees to find a *provably optimal* tree by minimizing a regularized objective function:

**Objective:** `R(d) = Misclassification Error + (λ * Number of Leaves)`

By balancing accuracy and sparsity (via the penalty parameter `λ`), the algorithm generates highly interpretable and accurate machine learning models suited for high-stakes decision-making.

## Features

*   **Custom Bit-Vector Operations:** Uses fast bitwise operations for efficient leaf support and accuracy calculations.
*   **Analytical Pruning Bounds:** Implements theorems from the paper to drastically reduce the search space, including:
    *   Hierarchical Objective Lower Bound
    *   One-Step Lookahead Bound
    *   Equivalent Points Bound
    *   Lower Bounds on Node Support and Accuracy
*   **Computational Caching:** Prevents redundant calculations by caching canonical leaf representations and pruned symmetrical trees.
*   **Priority Scheduling:** Utilizes a custom "Curiosity" metric to prioritize the priority queue, finding high-quality trees faster.
*   **Baseline Comparisons:** Integrates `scikit-learn`'s CART for greedy baseline comparisons and warm-starting the optimization loop.

## Repository Structure

```text
├── data/
│   └── heloc_dataset_v1.csv         # FICO HELOC dataset used for experiments
├── osdt.ipynb                       # Main Jupyter Notebook with implementation and experiments
├── osdt_presentation.html           # 5-slide scientific presentation of the project
└── README.md                        # Project documentation
```

## Requirements

The implementation is written in pure Python to demonstrate the core algorithmic concepts. The following libraries are required to run the notebook and baseline comparisons:

*   Python 3.7+
*   `numpy` (for data manipulation and bit-packing)
*   `pandas` (for data loading and cleaning)
*   `scikit-learn` (for CART baseline generation)
*   `matplotlib` (for plotting accuracy and convergence curves)

To install the dependencies, you can run:
```bash
pip install numpy pandas scikit-learn matplotlib
```

## Results

The algorithm was empirically tested on the **FICO HELOC** dataset. Key findings include:

1.  **Global Optimality:** OSDT successfully bypasses the pitfalls of greedy splitting. Under 5-fold cross-validation (at a fixed sparsity budget of 3 leaves), OSDT achieved a mean test accuracy of **71.13%**, outperforming CART's **70.67%**.
2.  **Effective Pruning:** The analytical bounds are critical for scalability. For example, disabling the *Equivalent Points Bound* on the FICO dataset causes the number of evaluated trees to explode from a mere 40 to over 87,000.
3.  **Interpretability:** By adjusting the regularization parameter `λ`, the algorithm successfully extracts highly transparent, certifiably optimal decision stumps and shallow trees suitable for human review.