# GSoC 2026 Proposal: Aligning pyGAM with Modern Scikit-Learn API Standards
**Candidate:** Hazem Mahmoud  
**Project:** Idea #2: Scikit-Learn Interface Integration  
**Mentor:** Daniel Servén  

## 1. Introduction
The objective of this project is to bridge the gap between `pyGAM` and the broader Python ML ecosystem. Currently, `pyGAM` models face challenges when integrated into `scikit-learn` workflows, particularly with cloning, hyperparameter tuning, and Pipeline compatibility. As a developer with a solid background in **Python OOP** and the **Scikit-learn** stack (`NumPy`, `Pandas`, `Matplotlib`), I propose a systematic refactoring to make `pyGAM` a first-class citizen in the sklearn universe.

## 2. Technical Audit & Problem Identification
After exploring the `pyGAM` source, I’ve identified specific blockers that my proposal aims to fix:
* **Estimator Contract:** `pyGAM` models need to strictly adhere to the `Scikit-learn` estimator contract (e.g., proper `get_params` and `set_params` through `BaseEstimator` inheritance).
* **The Initialization Trap:** Moving validation logic out of `__init__` to ensure that `sklearn.base.clone()` works reliably without pre-fitted state interference.
* **Input Validation:** Implementing standard `check_array` and `check_X_y` patterns to handle various data inputs (DataFrames, Sparse matrices, etc.) seamlessly.

## 3. Implementation Strategy

### Phase 1: Diagnostic & Baseline (Weeks 1-3)
* **Goal:** Use `sklearn.utils.estimator_checks.check_estimator` to generate a comprehensive list of non-compliant behaviors.
* **Focus:** Identifying issues in `LinearGAM`, `LogisticGAM`, and `PoissonGAM`.

### Phase 2: Refactoring & Inheritance (Weeks 4-8)
* **Core Work:** Refactor base classes to inherit from `BaseEstimator` and relevant Mixins. 
* **OOP Optimization:** Ensuring all hyperparameters are assigned to `self` in `__init__` without modification, a core requirement for sklearn compatibility.
* **Validation:** Updating `fit()` and `predict()` to match the expected sklearn return types and input handling.

### Phase 3: Ecosystem Integration (Weeks 9-12)
* **Testing:** Building test suites that run `pyGAM` models inside `GridSearchCV` and `RandomizedSearchCV`.
* **Deep Learning Context:** Leveraging my ongoing studies in `PyTorch` to ensure that the data-handling logic remains efficient and scalable.
* **Documentation:** Crafting clear examples of `pyGAM` used within an `sklearn.pipeline.Pipeline`.

## 4. Skills & Background
My expertise lies in the intersection of data manipulation and object-oriented design. 
* **Proficient in:** NumPy, Pandas, Scikit-learn, and Python OOP.
* **Current Focus:** Deep Learning with PyTorch, which has sharpened my understanding of complex model architectures and parameter management.
* **Philosophy:** I believe in "Clean Code" and "Test-Driven Development." I don't just want the code to work; I want it to pass the official sklearn "exam."

## 5. Proposed Timeline (175-Hour Format)
* **Phase 1 (25h):** Run diagnostic suites, identify failing "check_estimator" cases, and engage with the community for feedback.
* **Phase 2 (80h):** Major refactor of class definitions, `__init__` cleanup, and implementing `set_params`/`get_params`.
* **Phase 3 (40h):** Intensive testing with `sklearn.model_selection` and `Pipeline` integration.
* **Phase 4 (30h):** Final documentation, refactoring based on mentor feedback, and PR merging.
