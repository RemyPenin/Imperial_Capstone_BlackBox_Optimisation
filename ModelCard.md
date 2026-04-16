# Simplified Model Card – Black-Box Optimisation Capstone

This model card is separated by black-box function, as the notebook uses different surrogate models and acquisition settings depending on the function. 

---

## Function 1

### 1. Model overview
Model name: Function 1 optimiser  
Version: Current notebook version  
Developer: Rémy Penin  
Contact information: Not provided  
Licence: For academic / educational project use  

This model is a Bayesian Optimisation set-up for black-box function 1. It uses Gaussian Process Regression with a Rational Quadratic kernel and a white noise term. UCB is used as the acquisition function. A heteroskedasticity adjustment is added through an additional uncertainty term based on Random Forest spread. Ensembling across 5 random seeds is used. At later rounds, a more-trusted elliptical region is activated around the current best point.

### 2. Intended use
Primary task: Maximise black-box function 1  
Target users: Student, facilitators, recruiters or researchers reviewing the capstone project  
Recommended use cases: Expensive low-dimensional optimisation under a limited query budget  
Not recommended for: Classification tasks, high-frequency production decision systems, or problems where reliable broad global exploration is still required late in the process  

### 3. Training data
Data sources: Initial challenge data plus weekly newly observed points from the challenge process  
Size of data set: Initially 10 observations, then extended week after week in the notebook  
Languages or modalities: Numerical tabular data only  
Preprocessing steps: Conversion to NumPy arrays; global candidate generation using Sobol sequences; local candidate generation around the current best observation using Gaussian perturbations; duplicate avoidance; later restriction to a more-trusted elliptical region around the current best point. Rough candidate counts are around 20k–30k global candidates plus around 2k–5k local Gaussian candidates.  

### 4. Evaluation metrics
Metrics used: Actual black-box value obtained after each weekly submission, plus some hold-out error checks in the notebook  
Performance results: Improved relative to the initial data, but performance remains sensitive to exploration vs exploitation. This function is one of the functions for which I introduced both a heteroskedasticity adjustment and a more-trusted region.  
Fairness or bias checks (if any): No fairness checks. This is a numerical optimisation task, not a human-facing prediction task.  

### 5. Ethical considerations
Potential biases or risks: Search may be biased towards the neighbourhood of the current best point once the more-trusted region is activated, which may reduce the ability to find a better distant optimum. The heteroskedasticity adjustment may also put extra weight on uncertain regions depending on how well this uncertainty proxy reflects the actual behaviour of the function.  
Mitigation strategies: Earlier rounds relied much more on broad exploration; later rounds use a more local strategy only once more observations are available. Ensembling across random seeds may also reduce the impact of one unstable model initialisation.  
Privacy concerns: None from the data itself, as no personal data is used.  

### 6. Model life cycle
Date of last update: Latest attached notebook  
Version control or repository: Notebook-based capstone workflow  
Monitoring plan (if applicable): Weekly review of suggested point, actual returned value, and diagnostic plots.  

---

## Function 2

### 1. Model overview
Model name: Function 2 optimiser  
Version: Current notebook version  
Developer: Rémy Penin  
Contact information: Not provided  
Licence: For academic / educational project use  

This model is a Bayesian Optimisation set-up for black-box function 2. It uses ExtraTreesRegressor as the surrogate. UCB is used as the acquisition function. Uncertainty is approximated from ensemble spread across trees. Ensembling across 5 random seeds is used. A more-trusted elliptical region is activated in later rounds.

### 2. Intended use
Primary task: Maximise black-box function 2  
Target users: Student, facilitators, recruiters or researchers reviewing the capstone project  
Recommended use cases: Expensive optimisation with non-linear or less obviously smooth response surfaces  
Not recommended for: Tasks requiring fully calibrated probabilistic uncertainty or strong guarantees on global optimality  

### 3. Training data
Data sources: Initial challenge data plus weekly newly observed points from the challenge process  
Size of data set: Initially 10 observations, then extended week after week in the notebook  
Languages or modalities: Numerical tabular data only  
Preprocessing steps: Conversion to NumPy arrays; global candidate generation using Sobol sequences; local candidate generation around the current best observation using Gaussian perturbations; duplicate avoidance; later restriction to a more-trusted elliptical region around the current best point. Rough candidate counts are around 20k–30k global candidates plus around 2k–5k local Gaussian candidates.  

### 4. Evaluation metrics
Metrics used: Actual black-box value obtained after each weekly submission, plus some hold-out error checks in the notebook  
Performance results: Reasonably stable, with less need for additional heteroskedasticity correction than some of the other functions.  
Fairness or bias checks (if any): No fairness checks. This is a numerical optimisation task, not a human-facing prediction task.  

### 5. Ethical considerations
Potential biases or risks: Ensemble spread from trees is only a proxy for uncertainty, not a calibrated posterior uncertainty. Search may also become more locally biased once the more-trusted region is activated.  
Mitigation strategies: Earlier broad exploration, local refinement, and ensembling across random seeds may make the final suggestion more robust.  
Privacy concerns: None from the data itself, as no personal data is used.  

### 6. Model life cycle
Date of last update: Latest attached notebook  
Version control or repository: Notebook-based capstone workflow  
Monitoring plan (if applicable): Weekly update and review of surrogate behaviour and selected point.  

---

## Function 3

### 1. Model overview
Model name: Function 3 optimiser  
Version: Current notebook version  
Developer: Rémy Penin  
Contact information: Not provided  
Licence: For academic / educational project use  

This model is a Bayesian Optimisation set-up for black-box function 3. It uses Gaussian Process Regression with a Matérn kernel and a white noise term. EI is used as the acquisition function. Ensembling across 5 random seeds is used. A more-trusted elliptical region is activated in later rounds.

### 2. Intended use
Primary task: Maximise black-box function 3  
Target users: Student, facilitators, recruiters or researchers reviewing the capstone project  
Recommended use cases: Smooth, low-dimensional expensive optimisation problems  
Not recommended for: Problems where surrogate smoothness assumptions are clearly violated  

### 3. Training data
Data sources: Initial challenge data plus weekly newly observed points from the challenge process  
Size of data set: Initially 15 observations, then extended week after week in the notebook  
Languages or modalities: Numerical tabular data only  
Preprocessing steps: Conversion to NumPy arrays; global candidate generation using Sobol sequences; local candidate generation around the current best observation using Gaussian perturbations; duplicate avoidance; later restriction to a more-trusted elliptical region around the current best point. Rough candidate counts are around 30k–50k global candidates plus around 5k–10k local Gaussian candidates.  

### 4. Evaluation metrics
Metrics used: Actual black-box value obtained after each weekly submission, plus some hold-out error checks in the notebook  
Performance results: One of the more stable GP-based functions, with no major need for additional heteroskedasticity correction in the latest notebook.  
Fairness or bias checks (if any): No fairness checks. This is a numerical optimisation task, not a human-facing prediction task.  

### 5. Ethical considerations
Potential biases or risks: Heavy local search later in the process may bias the search around the current best region. GP uncertainty may also be imperfect if smoothness assumptions are not fully correct.  
Mitigation strategies: Earlier rounds relied on broader exploration; EI rather than UCB was used for this function; ensembling across random seeds may reduce instability from one model run.  
Privacy concerns: None from the data itself, as no personal data is used.  

### 6. Model life cycle
Date of last update: Latest attached notebook  
Version control or repository: Notebook-based capstone workflow  
Monitoring plan (if applicable): Weekly review of EI-driven suggestions and actual returned values.  

---

## Function 4

### 1. Model overview
Model name: Function 4 optimiser  
Version: Current notebook version  
Developer: Rémy Penin  
Contact information: Not provided  
Licence: For academic / educational project use  

This model is a Bayesian Optimisation set-up for black-box function 4. It uses ExtraTreesRegressor as the surrogate. UCB is used as the acquisition function. Uncertainty is approximated from ensemble spread across trees. Ensembling across 5 random seeds is used. A more-trusted elliptical region is activated in later rounds.

### 2. Intended use
Primary task: Maximise black-box function 4  
Target users: Student, facilitators, recruiters or researchers reviewing the capstone project  
Recommended use cases: Moderately higher-dimensional expensive optimisation where tree ensembles may be more robust than GPs  
Not recommended for: Tasks requiring fully calibrated probabilistic uncertainty or strong guarantees on global optimality  

### 3. Training data
Data sources: Initial challenge data plus weekly newly observed points from the challenge process  
Size of data set: Initially 30 observations, then extended week after week in the notebook  
Languages or modalities: Numerical tabular data only  
Preprocessing steps: Conversion to NumPy arrays; global candidate generation using Sobol sequences; local candidate generation around the current best observation using Gaussian perturbations; duplicate avoidance; later restriction to a more-trusted elliptical region around the current best point. Rough candidate counts are around 30k–60k global candidates plus around 5k–10k local Gaussian candidates.  

### 4. Evaluation metrics
Metrics used: Actual black-box value obtained after each weekly submission, plus some hold-out error checks in the notebook  
Performance results: Mixed but improving. At some stage I identified that local candidate density may have been too low in the more-trusted region and that this should be improved.  
Fairness or bias checks (if any): No fairness checks. This is a numerical optimisation task, not a human-facing prediction task.  

### 5. Ethical considerations
Potential biases or risks: Sampling density may bias the search more than model choice itself if the candidate pool is not fine enough. Ensemble spread from trees remains only a proxy for uncertainty.  
Mitigation strategies: Plan to regenerate denser candidates inside the more-trusted region; earlier rounds relied on broader exploration; ensembling across random seeds may reduce sensitivity to one model run.  
Privacy concerns: None from the data itself, as no personal data is used.  

### 6. Model life cycle
Date of last update: Latest attached notebook  
Version control or repository: Notebook-based capstone workflow  
Monitoring plan (if applicable): Weekly visual inspection of surrogate paths and selected candidates.  

---

## Function 5

### 1. Model overview
Model name: Function 5 optimiser  
Version: Current notebook version  
Developer: Rémy Penin  
Contact information: Not provided  
Licence: For academic / educational project use  

This model is a Bayesian Optimisation set-up for black-box function 5. It uses Gaussian Process Regression with an RBF kernel and a white noise term. EI is used as the acquisition function. A heteroskedasticity adjustment is added through an additional uncertainty term based on Random Forest spread. Ensembling across 5 random seeds is used. A more-trusted elliptical region is activated in later rounds. The target values are log-transformed in the notebook.

### 2. Intended use
Primary task: Maximise black-box function 5  
Target users: Student, facilitators, recruiters or researchers reviewing the capstone project  
Recommended use cases: Expensive optimisation where output scaling may be important and where some heteroskedasticity may be present  
Not recommended for: Problems where log transformation would be inappropriate  

### 3. Training data
Data sources: Initial challenge data plus weekly newly observed points from the challenge process  
Size of data set: Initially 20 observations, then extended week after week in the notebook  
Languages or modalities: Numerical tabular data only  
Preprocessing steps: Conversion to NumPy arrays; log transformation of outputs; global candidate generation using Sobol sequences; local candidate generation around the current best observation using Gaussian perturbations; duplicate avoidance; later restriction to a more-trusted elliptical region around the current best point. Rough candidate counts are around 30k–60k global candidates plus around 5k–10k local Gaussian candidates.  

### 4. Evaluation metrics
Metrics used: Actual black-box value obtained after each weekly submission, plus some hold-out error checks in the notebook  
Performance results: Improved after output transformation and later uncertainty refinement. This is one of the functions where heteroskedasticity adjustment seemed justified.  
Fairness or bias checks (if any): No fairness checks. This is a numerical optimisation task, not a human-facing prediction task.  

### 5. Ethical considerations
Potential biases or risks: Output transformation may change relative sensitivity across regions. Search may also become more locally biased once the more-trusted region is activated. The heteroskedasticity adjustment may put extra weight on uncertain regions depending on how well this uncertainty proxy reflects the actual behaviour of the function.  
Mitigation strategies: Earlier broad exploration, later local refinement only after more observations became available, and ensembling across random seeds.  
Privacy concerns: None from the data itself, as no personal data is used.  

### 6. Model life cycle
Date of last update: Latest attached notebook  
Version control or repository: Notebook-based capstone workflow  
Monitoring plan (if applicable): Weekly monitoring of EI suggestions, transformed output behaviour, and local uncertainty patterns.  

---

## Function 6

### 1. Model overview
Model name: Function 6 optimiser  
Version: Current notebook version  
Developer: Rémy Penin  
Contact information: Not provided  
Licence: For academic / educational project use  

This model is a Bayesian Optimisation set-up for black-box function 6. It uses ExtraTreesRegressor as the surrogate. UCB is used as the acquisition function. Uncertainty is approximated from ensemble spread across trees and refined using a more QRF-style quantile spread. A heteroskedasticity adjustment is also used. Ensembling across 5 random seeds is used. A more-trusted elliptical region is activated in later rounds.

### 2. Intended use
Primary task: Maximise black-box function 6  
Target users: Student, facilitators, recruiters or researchers reviewing the capstone project  
Recommended use cases: Expensive optimisation with non-linear or irregular response surfaces  
Not recommended for: Tasks requiring fully calibrated probabilistic uncertainty or strong guarantees on global optimality  

### 3. Training data
Data sources: Initial challenge data plus weekly newly observed points from the challenge process  
Size of data set: Initially 20 observations, then extended week after week in the notebook  
Languages or modalities: Numerical tabular data only  
Preprocessing steps: Conversion to NumPy arrays; global candidate generation using Sobol sequences; local candidate generation around the current best observation using Gaussian perturbations; duplicate avoidance; later restriction to a more-trusted elliptical region around the current best point. Rough candidate counts are around 80k–120k global candidates plus around 10k–20k local Gaussian candidates.  

### 4. Evaluation metrics
Metrics used: Actual black-box value obtained after each weekly submission, plus some hold-out error checks in the notebook  
Performance results: Reasonably robust, with moderate heteroskedasticity patterns that seemed to justify some uncertainty refinement but not an extreme correction.  
Fairness or bias checks (if any): No fairness checks. This is a numerical optimisation task, not a human-facing prediction task.  

### 5. Ethical considerations
Potential biases or risks: Ensemble disagreement may not correspond to true uncertainty. Search may also become more locally biased once the more-trusted region is activated.  
Mitigation strategies: Earlier broad exploration, local refinement, heteroskedasticity adjustment, and ensembling across random seeds may make the final suggestion more robust.  
Privacy concerns: None from the data itself, as no personal data is used.  

### 6. Model life cycle
Date of last update: Latest attached notebook  
Version control or repository: Notebook-based capstone workflow  
Monitoring plan (if applicable): Weekly review of tree-based surrogate behaviour and top proposed points.  

---

## Function 7

### 1. Model overview
Model name: Function 7 optimiser  
Version: Current notebook version  
Developer: Rémy Penin  
Contact information: Not provided  
Licence: For academic / educational project use  

This model is a Bayesian Optimisation set-up for black-box function 7. It uses ExtraTreesRegressor as the surrogate. UCB is used as the acquisition function. Uncertainty is approximated from ensemble spread across trees and strengthened because this function showed clearer increasing uncertainty near strong-value regions. Ensembling across 5 random seeds is used. A more-trusted elliptical region is activated in later rounds.

### 2. Intended use
Primary task: Maximise black-box function 7  
Target users: Student, facilitators, recruiters or researchers reviewing the capstone project  
Recommended use cases: Higher-dimensional expensive optimisation with non-linear or irregular response surfaces  
Not recommended for: Tasks requiring fully calibrated probabilistic uncertainty or strong guarantees on global optimality  

### 3. Training data
Data sources: Initial challenge data plus weekly newly observed points from the challenge process  
Size of data set: Initially 40 observations, then extended week after week in the notebook  
Languages or modalities: Numerical tabular data only  
Preprocessing steps: Conversion to NumPy arrays; global candidate generation using Sobol sequences; local candidate generation around the current best observation using Gaussian perturbations; duplicate avoidance; later restriction to a more-trusted elliptical region around the current best point. Rough candidate counts are around 100k–150k global candidates plus around 10k–20k local Gaussian candidates.  

### 4. Evaluation metrics
Metrics used: Actual black-box value obtained after each weekly submission, plus some hold-out error checks in the notebook  
Performance results: One of the more computationally intensive non-GPU functions. The move to a more-trusted region significantly improved practical results in later rounds.  
Fairness or bias checks (if any): No fairness checks. This is a numerical optimisation task, not a human-facing prediction task.  

### 5. Ethical considerations
Potential biases or risks: Tree-based uncertainty remains a proxy only, and the more-trusted region may bias the search strongly towards the current best neighbourhood.  
Mitigation strategies: Earlier broad exploration, later local refinement only after more observations became available, and ensembling across random seeds.  
Privacy concerns: None from the data itself, as no personal data is used.  

### 6. Model life cycle
Date of last update: Latest attached notebook  
Version control or repository: Notebook-based capstone workflow  
Monitoring plan (if applicable): Weekly review of surrogate diagnostics, acquisition behaviour, and actual returned values.  

---

## Function 8

### 1. Model overview
Model name: Function 8 optimiser  
Version: Current notebook version  
Developer: Rémy Penin  
Contact information: Not provided  
Licence: For academic / educational project use  

This model is a Bayesian Optimisation set-up for black-box function 8. It uses XGBoost as the surrogate, trained on GPU. UCB is used as the acquisition function. Uncertainty is approximated from disagreement across ensemble models trained with different random seeds. A more-trusted elliptical region is activated in later rounds.

### 2. Intended use
Primary task: Maximise black-box function 8  
Target users: Student, facilitators, recruiters or researchers reviewing the capstone project  
Recommended use cases: Higher-dimensional expensive optimisation where GPU-based tree boosting may keep runtime manageable  
Not recommended for: Tasks requiring fully calibrated probabilistic uncertainty or strong guarantees on global optimality  

### 3. Training data
Data sources: Initial challenge data plus weekly newly observed points from the challenge process  
Size of data set: Initially 40 observations, then extended week after week in the notebook  
Languages or modalities: Numerical tabular data only  
Preprocessing steps: Conversion to NumPy arrays; global candidate generation using Sobol sequences; local candidate generation around the current best observation using Gaussian perturbations; duplicate avoidance; later restriction to a more-trusted elliptical region around the current best point. Rough candidate counts are around 200k–300k global candidates plus around 20k–30k local Gaussian candidates.  

### 4. Evaluation metrics
Metrics used: Actual black-box value obtained after each weekly submission, plus some hold-out error checks in the notebook  
Performance results: GPU training kept runtime manageable. Ensemble-based uncertainty remained useful in practice, but less naturally interpretable than GP uncertainty.  
Fairness or bias checks (if any): No fairness checks. This is a numerical optimisation task, not a human-facing prediction task.  

### 5. Ethical considerations
Potential biases or risks: Ensemble disagreement for XGBoost is only an uncertainty proxy, not a calibrated posterior uncertainty. Search may also become more locally biased once the more-trusted region is activated.  
Mitigation strategies: Earlier broad exploration, later local refinement only after more observations became available, and ensembling across random seeds.  
Privacy concerns: None from the data itself, as no personal data is used.  

### 6. Model life cycle
Date of last update: Latest attached notebook  
Version control or repository: Notebook-based capstone workflow  
Monitoring plan (if applicable): Weekly review of acquisition behaviour, returned values, and whether exploration remains coherent with the surrogate surface.  

---

## General notes

The approach evolved from:
- strong global exploration  
to  
- more local optimisation using more-trusted regions  

Key improvements came from:
- reducing exploration (Kappa/Xi)  
- introducing heteroskedasticity adjustments where relevant  
- increasing local candidate density  
- restricting search space to a more-trusted region once enough observations were available  

Parameter tuning alone was not sufficient, and more structural changes were required to better control the optimisation process.
