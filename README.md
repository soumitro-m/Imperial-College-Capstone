1. Project overview
This project is the capstone project of the Professional Certificate in Machine Learning and Artificial Intelligence of Imperial College Business School. It mimics a Bayesian optimisation-style competition, i.e. there is a black-box function, with initial data points provided to participants who will build a statistical surrogate model of the black-box function and acquisitions function to submit multiple rounds of intelligent queries to find the optimum solutions, using Bayesian optimization technique and limited total number of queries. Such setting mirrors real-world machine learning problem, of which the analytical expression of the underlying problem is unknown (i.e. black-box function) and cost to find such expression is costly (thereby the total number of queries is limited given the cost). This project serves as a practice exercise for participants to apply skills and knowledge acquired in this Certificate program to real-world problems.

2. Data
In this project, there are 8 black-box functions to optimise.

Function 1: f: R2 ---> R
Detect likely contamination sources in a two-dimensional area, such as a radiation field, where only proximity yields a non-zero reading

Function 2: f: R2 ---> R
Imagine a black box, or a mystery ML model, that takes two numbers as input and returns a log-likelihood score. The goal is to maximise that score, but each output is noisy, and depending on where you start, you might get stuck in a local optimum.

Function 3: f: R3 ---> R
This is a drug discovery project, testing combinations of three compounds to create a new medicine. The inputs are the amounts of the three compounds used while the outputs are the number of adverse reactions (1D array). The goal is to minimise side effects; in this competition, it is framed as maximisation by optimising a transformed output (e.g. the negative of side effects).

Function 4: f: R4 ---> R
This task addresses the challenge of optimally placing products across warehouses for a business with high online sales, where accurate calculations are costly and only feasible biweekly. To speed up decision-making, an ML model approximates these results within hours. The model has four hyperparameters to tune, and its output reflects the difference from the expensive baseline. Because the system is dynamic and full of local optima, it requires careful tuning and robust validation to find reliable, near-optimal solutions.

Function 5: f: R4 ---> R
This task is to optimise a four-variable black-box function that represents the yield of a chemical process in a factory. The function is typically unimodal, with a single peak where yield is maximised. The goal is to find the optimal combination of chemical inputs that delivers the highest possible yield, using systematic exploration and optimisation methods.

Function 6: f: R5 ---> R
This task is to optimise a cake recipe using a black-box function with five ingredient inputs, for example flour, sugar, eggs, butter and milk. Each recipe is evaluated with a combined score based on flavour, consistency, calories, waste and cost, where each factor contributes negative points as judged by an expert taster. This means the total score is negative by design. To frame this as a maximisation problem, the goal is to bring that score as close to zero as possible or, equivalently, to maximise the negative of the total sum.

Function 7: f: R6 ---> R
This task is to optimise an ML model by tuning six hyperparameters, for example learning rate, regularisation strength or number of hidden layers. The function to be maximised is the model’s performance score (such as accuracy or F1), but since the relationship between inputs and output isn’t known, it’s treated as a black-box function.

Function 8: f: R8 ---> R
This task is to optimise an eight-dimensional black-box function, where each of the eight input parameters affects the output, but the internal mechanics are unknown. The objective is to find the parameter combination that maximises the function’s output, such as performance, efficiency or validation accuracy. Because the function is high-dimensional and likely complex, global optimisation is hard, so identifying strong local maxima is often a practical strategy. For example, imagine tuning an ML model with eight hyperparameters: learning rate, batch size, number of layers, dropout rate, regularisation strength, activation function (numerically encoded), optimiser type (encoded) and initial weight range. Each input set returns a single validation accuracy score between 0 and 1. The goal is to maximise this score.

3. Challenge objectives
The goal of this project is to find the maximum of these 8 black-box functions with limited total number of queries (i.e. 13) Major challenges and constraints of this project

Given limited background information about the functions and high dimensional inputs (which make visualisation hard or even impossible), it is difficult to find the right surrogate model to approximate the unknown functions
With limited number of queries, a balance between exploitation and exploration has to be stricken when deciding next query point
The high dimensionality of certain functions would mean
4. Technical approach
To solve the eight black-box optimisation problems, I adopted a sequential Bayesian Optimisation (BO) framework designed for data efficiency under limited query budgets. Since the analytical form of each function was unknown and evaluations were expensive (one query per function per week), a surrogate-assisted strategy was essential.

Primarily, I modelled the objective using a Gaussian Process (GP) surrogate. The GP provided both a predictive mean and uncertainty estimate, enabling principled decision-making under uncertainty. Kernels were carefully chosen based on visualization of outputs and background information provided, and certain hyperparameters were optimised via marginal likelihood maximisation.

In the initial phase, raw outputs were used directly. However, when certain functions exhibited sharp increases in objective values, kernel hyperparameter estimation became unstable. To address this, output normalisation was introduced to stabilise the optimisation landscape, improve length-scale estimation, and produce more reliable uncertainty quantification.

Next-point selection was primarily governed by an Upper Confidence Bound (UCB) acquisition function. Early iterations employed a relatively high exploration parameter (κ) to encourage global search across uncertain regions. As more observations were collected, κ was gradually reduced to prioritise exploitation of high-performing regions identified by the surrogate. This adaptive exploration–exploitation balance improved convergence stability.

Over successive iterations, the GP effectively acted as a structure detector, smoothing stochastic variability and identifying promising regions of the input space. Dense clusters of high-performing queries emerged in several functions, while low-value or inconsistent regions were progressively deprioritised. This iterative refine-and-update process was repeated weekly, allowing systematic improvement despite strict evaluation constraints.
