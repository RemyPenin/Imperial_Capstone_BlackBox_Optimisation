## Run the notebook (no installation required)

[![Open in Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/RemyPenin/Imperial_Capstone_BlackBox_Optimisation/main?labpath=CapstoneBBO.ipynb)

# Project Overview

This is one of the capstone projects for my Professional Certificate in Machine Learning and Artificial Intelligence at Imperial College. This is the Black-Box Optimisation (BBO) challenge that took place during the NeurIPS 2020 conference.

# Project Goal

The goal of this project is to maximize 8 black-box functions, using methods like Bayesian Optimisation (BO). The functions described real-world problems like radiations detection or drug discovery, in dimensions varying between 2 and 8, where evaluations are expensive or limited. These problems are not exactly classical machine learning challenges, more a global optimization under limited budget, but using machine learning methods. These are relevant practical challenges for various domains, notably where testing is expensive.

# Impact on My Career

I am upscaling my skills after a long career in quantitative finance. This challenge is the opportunity to apply the various techniques I am learning at Imperial College and adopt an approach that will be the one needed in future professional challenges I will encounter.

# Inputs and Outputs

For each of the 8 functions, inputs are a 2- to 8-dimension array, with initially between 10 and 40 data points (the number is higher as dimension increases). The outputs are the corresponding blackbox function values. Data set is clearly sparse for each of the 8 functions.

For instance, for function 1, an input is: `[0.727669, 0.734433]` and the corresponding output is `1.4957564730806974e-15`.

# What I Am Trying to Achieve With This BBO Challenge and What Are the Constraints and Limitations

As stated above, the goal is to find the global maximum of each of the 8 completely unknown functions. Data set is sparse, and each week I submit a new data point for each of the 8 functions and receive the corresponding actual values of the functions. There are 13 weeks, hence 13 submissions only. Again, testing is expensive.

# Strategies Used for the First 3 Submissions and Techniques Used, Including the Balancing Between Exploration and Exploitation

The BBO challenge started the week where we had the course about Bayesian Optimisation, which followed the courses about trees. So, my initial idea was to use what we had just learnt. Meanwhile, I already had in mind potential improvements that I put in place during the third week.

Implementation is in Python, notably using scikit-learn and xgboost packages (among others).

I wanted to adopt an approach where my pool of candidates for the next X value would be both covering uniformly the space and also focus more on the neighbourhood of the best existing observation. After some research, it seems that using a quasi-Monte-Carlo (i.e. not truly random, but more structured in a way to generate more uniformity) method like Sobol makes sense for the uniform candidate pool. Also, to avoid proposing redundant points, I introduced a minimum distance for this uniform pool from existing observations. For generating candidates close to the best existing observation, I just generated a random Gaussian noise around the best existing observation (local refinement). In the end, I built pools of between 20k+ (for function 1) and 300k+ (for functions 7 and 8) candidates.

In terms of surrogate functions, depending on the description of each blackbox function and on a bit of research about what it could mean (I have no expert knowledge in any of these areas), I made different modelling choices. For functions where the response was likely to be relatively smooth and continuous, I used Gaussian Process Regression with RBF or Matern kernels (functions 1, 3 and 5), as GPs naturally assumes the smoothness which is useful for Bayesian optimisation and provide a predictive uncertainty used directly by the acquisition function. For other functions where the behaviour could be a priori less smooth or more irregular, I used tree-based models (functions 2, 4, 6 and 7), as ensembles of decision trees seem to be more robust to non-linearities and not to rely on smoothness assumptions.

Finally, for function 8, which has the highest dimension, based on what was disclosed in the video about the Nvidia team that came second in the challenge, I wanted to explore the use of my Nvidia GPU. Trees seemed to be more relevant for function 8 as well, so I went for GPU XGBoost Regressor, using CUDA (GPU) as device instead of CPU. I did not formally benchmark CPU vs GPU here, but GPU training made the runtime manageable (around 1–2 min for function 8).

Then, in terms of kernels, where relevant, not knowing how smooth each function is, I combined either RBF (function 1 and 5) or Matern (function 3) with an explicit white noise term to improve numerical stability and account for potential noise.

For the acquisition function, to balance exploration and exploitation, I used either UCB (function 1 and all other functions using tree-based surrogates) or EI (functions 3 and 5). UCB was chosen in a way to encourage more exploration, with a kappa value in a 2.5–3.5 range.

Results were mixed for the first and second submissions, but I managed to rather improve the function value for several of the blackbox functions, without touching the still high exploration aspect. The only change I made between week 1 and week 2 was to use a log-value of the outputs for function 5 as values were very large for certain points.

For the third submission, I introduced ensembling of various initial values. Indeed, for results to be reproducible, I fixed a seed value for the randomness aspects. What I wanted was to do ensembling of 5 different seed values. Once the model is run with each seed value and returns a chosen point, I then chose the final point as the one that had the highest average acquisition score across the set of acquisition functions using each of the seed values. I kept the same exploration strength for the third week as well.

Results were more mixed for the third week. It could be the case that there was too much exploration, or it could be the case that it is just that exploration went past the optimum and would come back towards the maximum next week.

I wondered about using SVM and logistical regression (seen the weeks after BO). However, I chose not to:
- All functions are fundamentally regression problems rather than classification problems. So, logistical regression does not seem to be really relevant.
- For SVM, they could approximate promising regions, but would not optimise within them.

# Strategy for Weeks 4 to 13

## Week 4

For week 4, I slightly reduced exploration. At that stage, after three rounds, I felt that I might still have been too exploratory. The issue was that I could get an improvement one week, then a point that was much less interesting the next one. So, the idea was to keep the same overall framework, but start softening Kappa or Xi a bit depending on whether I was using UCB or EI.

## Week 5

For week 5, I kept refining the surrogate choices rather than changing the whole framework. I was still using the same general BO logic, but I started to focus more on whether the model itself was behaving as expected. This is also when I was progressively realising that some functions might not be handled equally well by the same type of surrogate.

## Week 6

For week 6, I kept the same broad approach, but I was increasingly trying to understand whether the issue was too much exploration or rather the surrogate itself. At that stage, I was still mostly adjusting kernels or model choices marginally, while keeping the same acquisition logic. I was not yet fully convinced that the problem was in the uncertainty term itself.

## Week 7

For week 7, I started to look much more carefully at the behaviour of the surrogate, notably through plots. This was an important stage, because I realised that some suggested points were far from the existing optimum along some features, in regions that did not seem that attractive according to the surrogate itself. So, even after reducing Kappa/Xi somewhat, the next suggestion could still be too far from where I wanted to exploit.

## Week 8

For week 8, I was still keeping a significant amount of exploration, but by then I had the impression that this was no longer helping enough. Yes, it may have been useful early on to discover new promising regions, but by that stage I was increasingly trusting the current best regions, especially for some functions. I was also starting to think more seriously about heteroskedasticity for some of the blackbox functions.

## Week 9

For week 9, I adjusted the acquisition functions for some functions to take heteroskedasticity into account through an additional term in the standard deviation used by UCB or EI. This was not needed for all functions, but for some of them it seemed that uncertainty was not constant across the search space, and in particular could be higher near the optimum. I think this was an important step, as reducing Kappa/Xi alone did not seem to be enough.

## Week 10

For week 10, I made a more structural change. Despite having reduced Kappa/Xi for previous submissions, I was still unhappy about the suggested next points. So, I introduced a greater focus around the current optimum by restricting the suggestions to an elliptical area around the current best point, i.e. a more-trusted region. This may be seen as moving from a more global search to a more local refinement phase. The choice of elliptical vs. spherical comes from the idea that spherical may be a lot more constraining in higher dimension and also when certain features have much more influence than others. So, the eligible area along each feature is defined in proportion to the standard deviation of the observed feature values for this feature. Otherwise, I was noticing that the next suggested points for some features could sometimes be very far from the feature value for the optimum, this despite having a higher density of candidates around the current optimum and the reduction in Kappa/Xi.

## Week 11

For week 11, I continued with the more local approach introduced in week 10, with a greater focus on the current best regions. At that stage, I was increasingly less convinced that broad exploration was still the best use of the remaining submissions. The goal was therefore to keep the benefits of the surrogate models and acquisition functions, but to make sure that the final suggestions remained close enough to the regions that already looked promising.

I also relied more heavily on diagnostic plots, especially the per-feature charts showing the model prediction and uncertainty along each feature. These plots were useful because the automated suggestion was sometimes not fully aligned with what I thought the structure of the function was suggesting. However, I still mostly kept the process model-driven and did not manually override the suggestions as much as I perhaps should have.

## Week 12

For week 12, I kept refining the exploitation phase. By then, most of the value of the project was less about discovering completely new regions, and more about improving around the best points already found. For some functions, especially function 4, I was unable to really improve from what appeared to be the optimum in the initially provided data. This may suggest either that the initial data already contained a very strong point, or that my model and candidate generation process were not able to identify a better region.

I continued to use tree-based models for functions 2, 4, 6 and 7, Gaussian Processes for functions 1, 3 and 5, and XGBoost for function 8. For some functions, I also kept the heteroskedasticity-style adjustment in the uncertainty term, as I felt that the standard uncertainty estimate alone was not always sufficient.

## Week 13

For the final week, the strategy was mostly focused on exploitation and risk control. With only one submission left, broad exploration seemed less justified, unless there was strong evidence that the current best region was not reliable. I therefore kept the candidate selection close to the trusted regions and used the diagnostic charts to assess whether the proposed points were plausible.

In hindsight, this final phase also highlighted one of the limitations of my approach. I remained somewhat unsatisfied with the automated suggestions for several functions, and my own feature-level charts sometimes suggested that alternative points might have been more sensible. Because I come from a systematic finance background, I was reluctant to intervene manually and override the model. For this challenge, however, a more discretionary review of the final suggested points might have helped.

# General Comments on the Evolution From Week 4 to Week 13

Overall, the strategy from week 4 to week 10 may be summarised as a progressive move from broad exploration towards more local exploitation. In the first rounds, I think the broad candidate generation and relatively high Kappa/Xi made sense, as there was little information and it was important to explore. But later, this may have become less efficient, and I increasingly focused on whether the suggested points were really coherent with the current best regions.

Another important evolution is that I moved from just adjusting parameters like Kappa/Xi to making more structural changes: first by adding an extra uncertainty component for some functions to account for heteroskedasticity, and then by explicitly restricting the search to a more-trusted local region. I think this was necessary, because parameter adjustments alone were not enough to get suggestions close enough to the current optimum when I wanted to exploit more.

In that sense, the project evolved from a rather standard BO framework with broad candidate generation and high exploration to a more refined local optimisation process, where the surrogate, the uncertainty, and the candidate region are all adjusted depending on the function and on the stage of the search. In the final weeks, the main question became whether to trust the automated suggestion or whether to intervene manually based on diagnostics.

If I had to restart the project, I would probably keep the same broad modelling split between GP, ExtraTrees and XGBoost, but I would tune the GP kernels more carefully earlier, use the diagnostic plots more systematically, and allow myself more manual intervention in the last submissions when the automated suggestions looked suboptimal.

# License

This project is licensed under the MIT License. The code can be freely used, modified and distributed, provided that the original copyright notice is retained.

See the LICENSE file for details.
