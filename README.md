[Readme.md](https://github.com/user-attachments/files/32933896/Readme.md)
**Black-Box Optimisation Challenge: Capstone Project README**

**Section 1: Project Overview**

This project is a Black-Box Optimisation (BBO) challenge designed to simulate real-world data science constraints where the underlying mechanisms of a system are hidden. The overall goal is to efficiently find the global maxima of multiple hidden target functions through iterative, evidence-based query submissions.

In real-world Machine Learning, BBO is relevant for high-stakes tasks like hyperparameter tuning of deep neural networks, drug discovery, and engineering design optimisation, where evaluating a single point is financially costly or computationally expensive. The high-level idea is to build an intelligent, data-driven "surrogate model" that learns from past evaluation data to guide where the next, most promising points should be sampled.

Engaging in this challenge directly supports my career development by mimicking the conditions faced by working data scientists: navigating incomplete knowledge and managing a strict experimentation budget.

**Section 2: Inputs and Outputs**

The optimisation framework operates on a structured query-and-response feedback loop. The model interacts with the hidden functions:

| **Function** | **Input array** | **Output array** |
| --- | --- | --- | --- |
| Function 1 | 2D | 1D |
| Function 2 | 2D | 1D |
| |
| Function 3 | 3D | 1D | |
| |
| |
| Function 4 | 4D | 1D | |
| Function 5 | 4D | 1D | |
| |
| Function 6 | 5D | 1D | |
| |
| Function 7 | 6D | 1D | |
| |
| Function 8 | 8D | 1D | |

- Inputs (Query Format): The model has a vector of feature coordinates representing a specific location within the search space.
  - _Dimensions:_ Varies by function, ranging from simple low-dimensional spaces (e.g., 2-dimensional for Functions 1 and 2) to highly complex, sparse, high-dimensional terrains (e.g., Function 8).
  - _Constraints:_ Bound constraints are enforced on all input features.
  - _Example Query Format:_ \[0.883889, 0.582253\], rounded to 6 decimal places
- Outputs (Response Value): The hidden function returns a scalar output corresponding directly to the submitted input vector. This output can be corrupted by an unknown level of stochastic system noise.
  - _Example Output Format:_ 6.229856e-48

**Section 3: Challenge Objectives**

The core objectives and structural limitations of this optimization challenge are defined as follows:

- Primary Objective: Maximize the scalar output scores across all hidden functions, identifying coordinates that yield the highest possible global maxima.
- Operational Constraints & Limitations:
  - Strict Query Budget: A limited total number of query rounds is permitted, preventing the use of brute-force sampling or fine-grained grids.
  - Response Delay: Evaluations happen in distinct batch iterations, requiring careful strategy formulation before receiving performance feedback.
  - Unknown Function Structures: The landscape geometries (smoothness, noise levels, and multimodality) are completely hidden, forcing the strategy to adapt to the incoming data.

**Section 4: Technical Approach**

ML Methods & Surrogate Modelling

The approach utilizes Bayesian Optimisation driven by a non-parametric Gaussian Process (GP) surrogate model. A Matérn kernel was deliberately selected over a standard Radial Basis Function (RBF) kernel to accommodate less smooth, rougher physical system behaviours. the GP provides exact, continuous predictive distributions and uncertainty bounds.

Exploration vs. Exploitation Balance

- Stage 1 (Baseline): Deployed a broad random initialization of 1,000 to 10,000 points within the feature bounds to map initial variance.
- Stage 2 (Adaptation): Used 3D plotting for straightforward landscapes (Functions 1 and 2) to pivot early toward local exploitation. For complex landscapes (Function 8), a hybrid focus leaning toward global exploration was maintained to prevent getting trapped in local optima.
- Stage 3 (Refinement): Transitioned to strict, data-driven optimization by tuning the acquisition function exploration parameter xi in Expected Improvement (EI).
