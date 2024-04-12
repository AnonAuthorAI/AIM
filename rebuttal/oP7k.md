**Thank you for viewing the detailed response we prepared for you! Your opinion is important to us to further improve the paper, and we are more than happy to address any further questions/concerns you may have. If our efforts have resolved your concerns, we sincerely hope and greatly appreciate your consideration in supporting the acceptance of our work.**

**We provide detailed responses to each of your questions below.**

- [Q1: Experiment on synthetic datasets with known bias](#q1-experiment-on-synthetic-datasets-with-known-bias)
- [Q2: Compare with more recent baselines and code release](#q2-compare-with-more-recent-baselines-and-code-release)

## Q1: Experiment on synthetic datasets with known bias

Thank you for the great suggestion. Following your advice, we have designed a series of new experiments with known ground truth bias to intuitively demonstrate the bias detection results and effectiveness of AIM. Per the reviewer's suggetion, we crafted four synthetic datasets with different types of bias to facilitate data visualization and precise evaluation of AIM's ability in capturing group- and individual-level bias.

Specifically, in each dataset, we sampled data of two groups from the same distribution, with Group #1 as the reference group, and introduced group/individual-level biases to Group #2. For group-level bias, we altered the class boundaries for the target group to simulate group-based discrimination (different groups having different "thresholds" for positive outcomes). For individual-level bias, we randomly selected 10% of samples from the target group and flipped their labels to simulate discrimination against specific individuals (similar individuals not being treated similarly). Please note that this approach provides ground truth labels for each sample's bias status, enabling us to visualize the quality of AIM bias attribution. We then apply AIM to calculate bias scores for each sample and label samples with scores > 0.5 as biased. 

The figure below presents our experimental results. Each row in the figure represents a synthetic dataset, and we label the bias type of the data at the beginning of each row. For each dataset, the first and second columns display the data and class distribution of groups #1 and #2, respectively. The third column shows the ground truth bias distribution on the target group (#2), where blue indicates unbiased and red indicates biased. In a simlar manner, the fourth column displays the sample bias distribution detected by AIM, with red indicating biased samples (with AIM bias score > 0.5). We also annotate the accuracy of AIM-detected biased samples w.r.t. the ground truth in the titles of the 4th-column subplots. It can be visually observed that AIM accurately detects group/individual-level bias in the data, with very high bias detection accuracy ranging from 97.0% to 99.8%.

![](https://raw.githubusercontent.com/AnonAuthorAI/AIM/main/figs/synthetic_bias_detection.png)

## Q2: Compare with more recent baselines and code release

Following your suggestion, we carefully investigated the literature and implemented three more recent FairML methods as additional baselines, all included in a recently published benchmark [1]. To ensure fair comparisons, we utilized the hyperparameters and experimental settings fine-tuned for each algorithm as per [1], ensuring optimal performance for each method. 

> We also noted some other FairML works, but they either did not provide the source code [2,3,4] or were designed for specific application scenarios [5-8], thus unsuitable as baselines for our work. We would greatly appreciate any specific recommendations regarding other baselines that should be considered.

Specifically, the additional baselines comprise FairMixup [9], which transforms fairness objectives into differentiable terms and optimizes them using gradient descent; AdvFair [10,11] (adversarial debiasing), which learns a classifier maximizing prediction ability while simultaneously minimizing an adversary's ability to predict sensitive attributes from predictions; and HSIC [12], which minimizes the Hilbert-Schmidt Independence Criterion between prediction accuracy and sensitive attributes. To comprehensively evaluate the performance of these methods, we followed the experimental settings in Table 1 and reported three utility metrics (ACC, ROC, AP), along with three metrics for individual and/or group (un)fairness: PC for individuals, EO for groups, and GE for both. The results are presented in the table below. Our key findings:
1. AIM effectively and simultaneously promotes both individual and group fairness, demonstrating a superior overall fairness-utility tradeoff compared to various baselines.
2. Among the new baselines, AdvFair typically exhibits the best utility but offers limited assistance in achieving group/individual fairness. FairMixup and HSIC offer better fairness assistance but with greater utility loss. Additionally, it's worth noting that HSIC is slower in practical operation, with training times approximately 20 times longer than other methods.

![](https://github.com/AnonAuthorAI/AIM/blob/main/figs/new_baselines.png?raw=true)

> References:  
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