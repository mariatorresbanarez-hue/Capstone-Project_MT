# Bayesian Black-Box Optimization (BBO) Capstone Project
\## Section 1: Project Overview



This project addresses the optimisation of eight unknown functions under a limited query budget. The functions are true black boxes: their formulas, gradients, and noise distributions are completely hidden, and each evaluation is costly. The goal is to learn from a small number of observations and propose progressively better sampling locations over time.



The approach is based on a Bayesian Optimisation workflow where the eight functions represent real-world problems:



| Function | Description | Objective Type |

|----------|-------------|-----------------|

| f1 | Detection of contamination sources (strong or weak) | Minimization (maximize negative) |

| f2 | Logarithmic likelihood score with noisy outputs | Maximization |

| f3 | Side effects / adverse reactions in drug discovery | Minimization |

| f4 | Cost minimization via optimal logistics | Minimization |

| f5 | Chemical process yield (optimal input combination) | Maximization |

| f6 | Expert taster scores (negative) | Maximization of the negative |

| f7 | Model performance by tuning 6 hyperparameters | Maximization |

| f8 | Validation accuracy / efficiency / performance (score 0–1) | Maximization |



BBO is relevant in real-world ML because most practical optimisation problems share the same core challenge: the objective function is unknown, each evaluation consumes significant resources, and exhaustive search is not feasible.



Real-world ML applications rarely allow unlimited experimentation, making it essential to learn as much as possible from every observation and decide intelligently where to sample next.



From a career perspective, this project directly supports my professional development as a Senior Instrumentation and Control Engineer working on large-scale mining and industrial projects.



Looking ahead, this capstone project strengthens my ability to design and propose advanced solutions for a wide range of processes, from industrial process control and plant optimization to intelligent automation systems and ML pipelines applied to complex engineering environments where information is incomplete, evaluations are costly, and every decision must be backed by solid evidence.



\## Section 2: Inputs and Outputs



The project involves optimization for eight different functions with varying dimensions in their inputs and output.



\### Inputs (features)



\*\*Initial data points\*\*



Format: `.npy` file



| Function | Data Points | Dimensions | Shape |

|----------|-------------|------------|-------|

| Function 1 | 10 | 2D | `(10, 2)` |

| Function 2 | 10 | 2D | `(10, 2)` |

| Function 3 | 15 | 3D | `(15, 3)` |

| Function 4 | 30 | 4D | `(30, 4)` |

| Function 5 | 20 | 4D | `(20, 4)` |

| Function 6 | 20 | 5D | `(20, 5)` |

| Function 7 | 30 | 6D | `(30, 6)` |

| Function 8 | 40 | 8D | `(40, 8)` |



Example 1 — input format (2D function): `\[0.31940389, 0.76295937]`



\### Outputs (target)



A single floating-point scalar value (performance signal or evaluation score) returned by the black-box function at those coordinates.



Example 1 — output format (2D function): `\[-1.32267704e-079]`



\### Submission of input values



Each week, I submit the proposed sampling points through the capstone portal.



\### Reception of output values (each week)



Each week, I receive the corresponding output values for my submitted inputs.



Format: `.txt` file



\## Section 3: Challenge Objectives



The goal is to maximize the 8 functions, which reflect the complexities of the real world. In some cases, depending on the nature of the problem, we need to find a minimum; in such cases, we use the negated function, thereby turning it into a maximization problem — for example, function 3 and function 6.



The main constraints are:



\- \*\*Limited query budget.\*\* Additional information is only the score returned by the portal after each submission.

\- \*\*Noisy outputs\*\* in some functions, but the noise structure is completely hidden.

\- \*\*Evidence-based sampling.\*\* Decisions about where to sample next must be evidence-based; random or exhaustive search is not viable.

\- \*\*Diverse functions.\*\* The eight functions vary in dimensionality (2D to 8D) and behaviour, meaning no single strategy or parameter set is guaranteed to work across all of them.



\## Section 4: Technical Approach



\### Strategies across query submissions



| Week | Phase | Kernel | Model | Surrogate model(s) |

|------|-------|--------|-------|---------------------|

| Week 1 | Exploration | RBF (l: length\_scale) | Gaussian Process (GP) (RBF, noise) | Expected Improvement — EI (ξ) |

| Week 2 | Exploration / Exploitation | RBF (l: dynamic length\_scale) | Gaussian Process (GP) (RBF, noise) | EI (ξ), Upper Confidence Bound — UCB (κ) |

| Week 3 | Exploration / Exploitation | RBF (l: dynamic length\_scale) | Gaussian Process (GP) (RBF, noise) + Response Surface Methodology (RSM) with simplified Gradient Descent | EI (ξ), UCB (κ) |



Notes:



\- \*\*Week 1\*\* also included an initial analysis of the received data using graphs and heat maps (F1, F2, F3).

\- Details of the tests conducted using different parameters, for each week, are available in the Excel file on my GitHub.



\### Q\&A



\*\*Would you consider using SVMs, regressions, or Bayesian techniques?\*\*



SVMs could be considered an alternative surrogate model (in place of GP), as they also handle nonlinear relationships. However, SVMs do not provide uncertainty estimates (σ(x)), so there is no systematic way to balance exploration and exploitation.



Regression models (linear, polynomial, RSM) could approximate the response surface, but they assume a parametric form that may not capture the complexity of black-box functions, especially in higher dimensions (6D–8D).



Bayesian techniques (specifically GP) were chosen because they:



\- explicitly quantify uncertainty through the posterior standard deviation σ(x)

\- update beliefs sequentially as new observations arrive

\- require fewer evaluations than other models

\- make no assumptions about the shape of the function



The use of RSM could be considered for lower dimensions, which would require further analysis of behavior in response to parameter changes.



\*\*How do you balance exploration and exploitation?\*\*



The balance is controlled through the acquisition function parameters and the kernel, adapted to the dimensionality of each function. The strategy is adaptive throughout the iterations, beginning with an exploration phase; once potentially optimal regions are detected, exploitation continues.



\*\*What makes your approach thoughtful or unique?\*\*



Given my limited experience with data analysis, I've been experimenting with different parameters until I find the most appropriate ones. This process has given me valuable practice in understanding the implications of parameter changes — specifically, how to stay within a specific range to achieve a more optimal response with a high mean and smaller standard deviation. 

