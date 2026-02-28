# About this Project
This project serves as the capstone for the Professional Certificate in Machine Learning and Artificial Intelligence at Imperial College Business School. It is designed as a Bayesian optimization–style challenge, where participants interact with a black-box function. Starting from a limited set of initial data points, participants develop statistical surrogate models and acquisition functions to intelligently propose queries across multiple rounds, with the goal of identifying the optimal solution using Bayesian optimization under a constrained query budget.

This setup reflects real-world machine learning scenarios, where the true analytical form of the underlying system is unknown (a black-box problem) and exploring it is expensive, making data acquisition limited and costly. The project provides a practical, hands-on opportunity for participants to apply the skills and knowledge gained throughout the certificate program to realistic, industry-relevant problems.

# Project Data
There are 8 Black Box functions to be optimized:
1. Function 1: 2 parameters
2. Function 2: 2 parameters
3. Function 3: 3 parameters
4. Function 4: 4 parameters
5. Function 5: 4 parameters
6. Function 6: 5 parameters
7. Function 7: 6 parameters
8. Function 8: 8 parameters

# Objectives
The objective of this project is to identify the maxima of eight black-box functions using a strictly limited query budget (13 total queries). This task involves several key challenges and constraints:

Modeling uncertainty: With minimal prior knowledge of the underlying functions and high-dimensional input spaces (which make visualization difficult or even infeasible), selecting an appropriate surrogate model to accurately approximate these unknown functions is inherently challenging.

Exploration vs. exploitation trade-off: The small number of allowed queries requires carefully balancing exploration of the search space with exploitation of promising regions when choosing each subsequent query point.

High-dimensional complexity: The dimensionality of some functions further increases the difficulty of optimization, compounding the challenges of model selection, search efficiency, and convergence.

# My approach
To address the eight black-box optimization problems, I implemented a sequential Bayesian Optimization (BO) framework specifically designed for data efficiency under a constrained query budget. Given that the analytical form of each function was unknown and evaluations were costly (limited to one query per function per week), a surrogate-assisted approach was essential.

Each objective was primarily modeled using a Gaussian Process (GP) surrogate, which provides both predictive means and uncertainty estimates, enabling principled decision-making under uncertainty. Kernel functions were selected based on exploratory visualization of outputs and the background information provided, with key hyperparameters optimized via marginal likelihood maximization.

In the early stages, raw outputs were used directly. However, for functions exhibiting sharp increases in objective values, kernel hyperparameter estimation became unstable. To mitigate this, output normalization was introduced to stabilize the optimization landscape, improve length-scale estimation, and enhance the reliability of uncertainty quantification.

Query selection was driven mainly by an Upper Confidence Bound (UCB) acquisition function. Initial iterations used a relatively high exploration parameter (κ) to promote broad exploration of uncertain regions. As observations accumulated, κ was progressively reduced to shift focus toward exploitation of high-performing regions identified by the surrogate model. This adaptive balance between exploration and exploitation improved convergence stability.

Across iterations, the GP effectively functioned as a structure-learning mechanism, smoothing stochastic variability and revealing promising regions within the input space. High-performing query clusters emerged for several functions, while low-value or inconsistent regions were systematically deprioritized. This iterative refine-and-update process was repeated weekly, enabling consistent improvement despite strict evaluation constraints.
