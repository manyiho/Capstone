**Model Card: Bayesian Optimisation**

1\. Overview

- Model Name: Bayesian Optimisation (BO)
- Model Type: Non-Parametric Bayesian Global Optimiser
- Version: v1.0 (Final Capstone Release)
- Core Architecture: Gaussian Process (GP) using Matérn Kernel.

2\. Intended Use

- Suitable Tasks:
  - Sequential, sample-efficient black-box maximisation and minimisation.
  - Derivative-free optimization in high-stakes environments with limited evaluation budgets (e.g., hyperparameter tuning of deep neural networks, chemical compound discovery, or physical system calibration).
  - Handling noisy, multimodal landscapes where feature cross-interactions heavily dominate performance.
- Use Cases to Avoid:
  - Cheap, high-throughput functions where evaluations are fast and plentiful (standard gradient descent or evolutionary algorithms are far more computationally efficient here).
  - Target landscapes with hard step-discontinuities or non-stationary noise distributions that violate spatial smoothness.

3\. Details & Strategic Evolution

The architecture evolved systematically across several discrete evaluation rounds, transforming from a naive, unguided sampler into a structured representation-learning framework:

- - _Phase 1:_ Broad baseline exploration using a random initialization of 1,000 to 10,000 virtual seeds to map initial variance.
    - _Phase 2:_ Transitioned to a Gaussian Process (GP) Matérn kernel running an internal multi-start Expected Improvement (EI) optimization.
    - _Phase 3:_ Exploitation with![](data:image/gif;base64,R0lGODlhAQABAPAAAP///wAAACH5BAEAAAAALAAAAAABAAEAQAICRAEAOw==)𝜉 down to ![](data:image/gif;base64,R0lGODlhAQABAPAAAP///wAAACH5BAEAAAAALAAAAAABAAEAQAICRAEAOw==)\=0.0001 to maximize local peaks.

4\. Performance Summary

- Metrics Used: Optimization Velocity (rate of output improvement per query batch), and Top Scalar Output Scores.
- Results across the Landscapes:
  - _Functions 1 & 2 (Low-Dim):_ Hit absolute global maxima early in Phase 2, displaying immediate flattening of the optimization velocity curve.
  - _Functions 4, 5, & 6 (Highly Multimodal):_ Achieved outstanding breakthrough milestones during Phase 3, successfully navigating steep geometric walls and tuned localized exploitation.
  - _Function 8 (High-Dim):_ Displayed steady, linear performance scaling through the Rounds.

5\. Assumptions and Limitations

- Underlying Assumptions: The model fundamentally assumes landscape stationarity and local smoothness. By operating with a Matérn kernel, it presumes that coordinates in spatial proximity yield highly correlated outputs.
- Failure Modes & Constraints:
  - _Outlier Blinding:_ If a hidden function contains an isolated "needle-in-a-haystack" spike or a vertical cliff, the GP's likelihood smoothing will misclassify the point as stochastic system noise and smooth completely past it.

6\. Ethical Considerations & Reproducibility

- Transparency as an Engineering Safeguard: In industrial data science, unguided trial-and-error costs immense computational, environmental, and financial capital. This model card ensures full algorithmic accountability.
- Reproducibility: By defining clear programmatic audit flags (such as asserting optimizer convergence status and locking random evaluation seeds), this framework allows future practitioners to cleanly duplicate our search trajectory and verify performance metrics.

7\. Structure & Architectural Sufficiency

It provides an external reviewer, peer, or future employer with the exact mechanistic blueprint needed to understand _why_ the model makes a decision (balancing exploration and exploitation via continuous hyperparameters).