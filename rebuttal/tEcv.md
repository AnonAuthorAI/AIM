**Thank you for viewing the detailed response we prepared for you! Your opinion is important to us to further improve the paper, and we are more than happy to address any further questions/concerns you may have. If our efforts have resolved your concerns, we sincerely hope and greatly appreciate your consideration in supporting the acceptance of our work.**

**We provide detailed responses to each of your questions below.**

- [Q1: Why fairness from a data-centric perspective?](#q1-why-fairness-from-a-data-centric-perspective)
  - [Unfairness is usually rooted in the data rather than the model](#unfairness-is-usually-rooted-in-the-data-rather-than-the-model)
  - [AIM not only mitigates but also explains unfairness](#aim-not-only-mitigates-but-also-explains-unfairness)
  - [AIM offers better interpretability and auditability than model-based FairML](#aim-offers-better-interpretability-and-auditability-than-model-based-fairml)
  - [AIM can also be applied to "various data with possible bias", and is model-agnostic](#aim-can-also-be-applied-to-various-data-with-possible-bias-and-is-model-agnostic)
- [Q2: Rationale of our unfairness mitigation strategy](#q2-rationale-of-our-unfairness-mitigation-strategy)
  - [Real or synthetic data?](#real-or-synthetic-data)
  - [Inveracious data?](#inveracious-data)
  - [Are "real" samples necessarily "helpful"?](#are-real-samples-necessarily-helpful)
- [W1: Regarding symbol definitions](#w1-regarding-symbol-definitions)
- [W2: Compare with more recent baselines](#w2-compare-with-more-recent-baselines)
- [W3: Remarks on the method design principle](#w3-remarks-on-the-method-design-principle)

## Q1: Why fairness from a data-centric perspective?

We would like to briefly explain why we chose to study fairness from a data-centric perspective from the following four aspects:

### Unfairness is usually rooted in the data rather than the model

There is a wide concensus in the literature that data is usually the root problem in FairML, here we quote some statements from some recent popular surveys: "*In the cases where the underlying training data contains biases, the algorithms trained on them will learn these biases*" [1]; "*intrinsic biases in the sample will manifest themselves in any model built on the data*" [2]; "*ML models are designed to learn and replicate historical patterns in the data, even if such patterns are biased*" [3]. Therefore, approaching fairness from the data level has its unique value in that it directly addresses the root cause of the unfairness problem, leading to FairML techniques that are both generalizable, and understandable/auditable by humans (as we will discuss below).

### AIM not only mitigates but also explains unfairness

Model-based FairML techniques are designed for mitigating unfairness, but often do not help users understand why bias exists in the data or which samples exhibit bias. By comparison, AIM can also serve as an explainer, providing human experts with additional insights about the pre-existing bias in the data through bias attribution and interpretation.

### AIM offers better interpretability and auditability than model-based FairML
Model-based FairML often mitigates unfairness by adding additional regularization terms or constraints, limiting users to assess algorithm and hyperparameter settings solely based on metrics on the validation set. AIM, on the other hand, directly conducts minimal sample-level data editing to mitigate unfairness, allowing users to directly inspect the deleted/added data samples to audit the algorithm's fairness.

### AIM can also be applied to "various data with possible bias", and is model-agnostic
As you correctly mentioned regarding the "fairness model of the research", AIM can similarly be applied to "various data with possible bias," as demonstrated in our paper (e.g., our main results Figure 2, Table 1, and Figure 5). Additionally, AIM is model-agnostic as it does not rely on modifying specific model optimization objectives or architectures, but directly conducts bias discovery and debiasing based on the data itself.

> **References**  
> [1] Mehrabi, Ninareh, et al. "A survey on bias and fairness in machine learning." ACM computing surveys (CSUR) 54.6 (2021): 1-35.  
> [2] Caton, Simon, and Christian Haas. "Fairness in machine learning: A survey." ACM Computing Surveys (2020).  
> [3] Pessach, Dana, and Erez Shmueli. "A review on fairness in machine learning." ACM Computing Surveys (CSUR) 55.3 (2022): 1-44.  

## Q2: Rationale of our unfairness mitigation strategy

### Real or synthetic data?

The training datasets processed through AIM_REM and AIM_AUG are **real data debiased with minimal (and thus auditable) sample-level manipulation**. Specifically, a few discriminatory/non-discriminatory **training** samples are removed/added to the original dataset. We do NOT change the test set, so the test results reflect AIM's ability to achieve significant fairness improvements with minimal or zero utility loss on **unseen real data**.

### Inveracious data?

Our main results in Figure 1 reflects that AIM effectively preserves data fidelity and does not lead to inveracious data, as it can achieve prediction quality close to or even better than the ERM model trained on the original data.

### Are "real" samples necessarily "helpful"?

Additionally, we want to point out that **a sample being "real" does not necessarily mean it is "helpful"** in terms of model utility and/or fairness. For example, instances of denying loans to qualified black applicants due to racial discrimination objectively exist (i.e., "real"), yet the inclusion of such discriminatory samples during training will evitably lead to a biased model. 

In other words, the goal of AIM_REM/AUG is to mitigate the negative impact of such real but toxic samples on training the model. Since only a small number of samples need to be removed/added to achieve fairness (please see Figure 3 for an example), AIM results in minimal changes to the original data distribution. This also allows human experts to audit the rationality of the operations performed on each affected sample.

## W1: Regarding symbol definitions

Thank you for your thorough review of our paper. We will carefully check and revise all notations to avoid any confusion. Currently, we are using italic capital letters to denote random events and regular capital letters to denote groups. While they are different, we agree with your concern that this might lead to potential confusion, and will revise them to improve clarity.

## W2: Compare with more recent baselines

Following your suggestion, we carefully investigated the literature and implemented three more recent FairML methods as additional baselines, all included in a recently published benchmark [1]. To ensure fair comparisons, we utilized the hyperparameters and experimental settings fine-tuned for each algorithm as per [1], ensuring optimal performance for each method. 

>We also noted some other FairML works, but they either did not provide the source code [2,3,4] or were designed for specific application scenarios [5-8], thus unsuitable as baselines for our work. We would greatly appreciate any specific recommendations regarding other baselines that should be considered.

Specifically, the additional baselines comprise FairMixup [9], which transforms fairness objectives into differentiable terms and optimizes them using gradient descent; AdvFair [10,11] (adversarial debiasing), which learns a classifier maximizing prediction ability while simultaneously minimizing an adversary's ability to predict sensitive attributes from predictions; and HSIC [12], which minimizes the Hilbert-Schmidt Independence Criterion between prediction accuracy and sensitive attributes. To comprehensively evaluate the performance of these methods, we followed the experimental settings in Table 1 and reported three utility metrics (ACC, ROC, AP), along with three metrics for individual and/or group (un)fairness: PC for individuals, EO for groups, and GE for both. The results are presented in the table below. Our key findings:
1. AIM effectively and simultaneously promotes both individual and group fairness, demonstrating a superior overall fairness-utility tradeoff compared to various baselines.
2. Among the new baselines, AdvFair typically exhibits the best utility but offers limited assistance in achieving group/individual fairness. FairMixup and HSIC offer better fairness assistance but with greater utility loss. Additionally, it's worth noting that HSIC is slower in practical operation, with training times approximately 20 times longer than other methods.


![](https://github.com/AnonAuthorAI/AIM/blob/main/figs/new_baselines.png?raw=true)

> **References:**  
> [1] Han, Xiaotian, et al. "FFB: A Fair Fairness Benchmark for In-Processing Group Fairness Methods." The Twelfth International Conference on Learning Representations. 2023.  
> [2] Xiong, Zikai, et al. "Fairwasp: Fast and optimal fair wasserstein pre-processing." Proceedings of the AAAI Conference on Artificial Intelligence. Vol. 38. No. 14. 2024.  
> [3] Jung, Sangwon, et al. "Re-weighting Based Group Fairness Regularization via Classwise Robust Optimization." The Eleventh International Conference on Learning Representations. 2022.  
> [4] Zhang, Wenbin, Tina Hernandez-Boussard, and Jeremy Weiss. "Censored fairness through awareness." Proceedings of the AAAI conference on artificial intelligence. Vol. 37. No. 12. 2023.  
> [5] Ezzeldin, Yahya H., et al. "Fairfed: Enabling group fairness in federated learning." Proceedings of the AAAI Conference on Artificial Intelligence. Vol. 37. No. 6. 2023.  
> [6] Wang, Ji, et al. "Scalable spectral clustering with group fairness constraints." International Conference on Artificial Intelligence and Statistics. PMLR, 2023.  
> [7] Ashktorab, Zahra, et al. "Fairness Evaluation in Text Classification: Machine Learning Practitioner Perspectives of Individual and Group Fairness." Proceedings of the 2023 CHI Conference on Human Factors in Computing Systems. 2023.  
> [8] Feng, Raymond, Flavio Calmon, and Hao Wang. "Adapting Fairness Interventions to Missing Values." Advances in Neural Information Processing Systems 36 (2024).  
> [9] Ching-Yao Chuang and Youssef Mroueh. Fair mixup: Fairness via interpolation. In ICLR, 2020.  
> [10] Tameem Adel, Isabel Valera, Zoubin Ghahramani, and Adrian Weller. One-network adversarial fairness. In Proceedings of the AAAI, pages 2412–2420, 2019.  
> [11] Zhang, Brian Hu, Blake Lemoine, and Margaret Mitchell. "Mitigating unwanted biases with adversarial learning." Proceedings of the 2018 AAAI/ACM Conference on AI, Ethics, and Society. 2018.
> [12] Sina Baharlouei, Maher Nouiehed, Ahmad Beirami, and Meisam Razaviyayn. Rényi fair inference. In International Conference on Learning Representations, 2020  

## W3: Remarks on the method design principle

We would like to concisely summarize the rationale behind our design of AIM$_{\text{REM/AUG}}$: we believe the most crucial properties of a practical unfairness mitigation method should include simplicity, effectiveness, and auditability. Therefore, we design them as natural extensions of the proposed AIM bias attribution method, capable of efficiently mitigating unfairness by removing or adding few samples. 

While we agree that exploring more advanced unfairness mitigation techniques (e.g., automatically discovering and accounting for feature-level discrimination) is an interesting future direction, they come with challenges such as handling categorical features, requiring extensive training data, and significant computational overhead.