# Datasheet: NeurIPS 2020 Black-Box Optimisation Challenge

## Motivation

The data set associated with the NeurIPS 2020 Black-Box Optimisation Challenge was created to support research and benchmarking in black-box optimisation. The core task is to optimise unknown objective functions under a limited number of queries, without access to gradients or internal structure.

The challenge may be seen as addressing a gap between theoretical optimisation methods and practical, real-world problems where the objective function is expensive, noisy or inaccessible. It may also aim at comparing different optimisation strategies (e.g. Bayesian optimisation, evolutionary methods) in a controlled yet diverse environment.

The data set and benchmark functions were created by the challenge organisers, likely a combination of academic researchers and industry practitioners, with the broader goal of advancing optimisation techniques. It is possible that the initiative was supported indirectly by research institutions or industry partners, although the primary motivation appears to be scientific benchmarking rather than commercial deployment.

---

## Composition

The data set does not consist of a traditional static collection of labelled instances. Instead, it is composed of a set of black-box objective functions that participants can query. These functions may represent synthetic benchmarks or surrogates of real-world problems.

Each “instance” may therefore be defined as:
- a function to optimise  
- a domain (search space)  
- a budget of evaluations  

The functions may vary in:
- dimensionality  
- smoothness  
- noise level  
- presence of local optima  

There is no explicit notion of labels in the supervised learning sense. Instead, each query returns a function value. The data set may therefore be seen as interactive rather than static.

The completeness of the benchmark may be questioned, as it is necessarily a subset of all possible optimisation problems. It may not fully represent real-world complexity, especially in domains with strong constraints or highly structured dependencies.

There are no direct privacy concerns, as the data does not represent individuals. However, some functions may indirectly reflect real-world inspired scenarios.

Recommended splits may exist in the form of:
- public functions for development  
- hidden test functions for evaluation  

This separation is important to prevent overfitting to known benchmarks.

---

## Collection process

The functions were likely generated using a mix of:
- synthetic benchmark generators  
- transformations of known optimisation problems  
- possibly surrogate models of real-world systems  

The sampling process is therefore not random in the traditional sense, but rather designed to cover a range of difficulties and characteristics.

The time frame of collection is tied to the preparation of the challenge (around 2020).

There is no direct human data collection, so ethical concerns related to consent or personal data are minimal. However, there may still be design choices that influence fairness across methods (e.g. favouring certain optimisation techniques).

The impact on participants is mainly methodological, as the benchmark may shape which algorithms are perceived as state-of-the-art.

---

## Preprocessing / cleaning / labelling

Preprocessing is minimal in the traditional sense, as the “data” consists of callable functions.

However, some transformations may have been applied:
- normalisation of input domains  
- scaling of outputs  
- addition of noise  

There is no labelling process, but the evaluation protocol (e.g. number of allowed queries, performance metrics) plays a similar role in structuring the task.

It is unclear whether the raw underlying functions (if derived from real-world data) are preserved or accessible.

---

## Uses

The primary use is benchmarking optimisation algorithms under controlled conditions. This may include:
- comparing Bayesian optimisation methods  
- testing exploration vs exploitation strategies  
- evaluating robustness to noise and dimensionality  

The data set may also be used for educational purposes or for developing new optimisation techniques.

However, there are limitations:
- performance on these benchmarks may not generalise to real-world problems  
- algorithms may overfit to the structure of the benchmark functions  

There may also be implicit biases in the choice of functions, favouring certain classes of algorithms. This could lead to misleading conclusions about real-world performance.

The data set should therefore be used with caution when drawing broader conclusions.

---

## Distribution

The data set was distributed as part of the NeurIPS challenge, likely through:
- online platforms  
- competition interfaces (APIs or evaluation servers)  

Access may have been open to participants during the competition period.

Licensing terms may depend on the specific implementation, but the intent appears to be academic and research-oriented use.

Some components (e.g. test functions) were likely kept private to ensure fair evaluation.

---

## Maintenance

Maintenance of the data set is likely limited after the challenge period.

The organisers may retain control over:
- evaluation servers  
- hidden test functions  
- benchmark definitions  

However, long-term updates or versioning may be limited, as such challenges are often one-off events.

This may reduce reproducibility over time if infrastructure is not maintained. On the other hand, the static nature of the benchmark may ensure consistency for comparisons across methods developed shortly after the challenge.

---

## Final remark

This data set may be seen less as a traditional data collection and more as a benchmarking framework. While it offers a controlled environment for evaluating optimisation methods, its representativeness and long-term maintenance may be limited. As such, it should likely be complemented with real-world validation before drawing strong conclusions.