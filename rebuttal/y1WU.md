Thank you for your insightful review and constructive comments of our paper. We respond to each of your concern below.

- ## Q1: How does AIM_AUG works?

AIM_AUG augments fair/low-bias samples by generating semantically consistent synthetic data points through neighborhood mixup, rather than simply adding identical samples (which may increase the risk of overfitting) or adding arbitrary perturbations to the original features (which may generate unrealistic data samples). Specifically, we first select a low-bias sample as our seed for augmentation. Then, we choose a target sample similar to the seed (i.e., in the neighborhood of the seed), and mixup the two in both numerical and categorical feature spaces to obtain a new synthetic sample. Intuitively, AIM_AUG avoids overfitting and unrealistic data generation by performing neighborhood mixup between real samples.

- ## Q2: Regarding how AIM can help human expert to investigate data bias

Thank you for your feedback and we fully agree with you: the inherent interpretability of bias is a unique advantage of our work. We will revise the paper to further emphasize this point and incorporate more relevant discussions. For instance, we believe that AIM can offer insights to human experts in the following aspects: 
1. Sample-level: Do the detected unfair cases comply with current regulations? Can human experts modify them to remove bias while retaining other information beneficial for model learning?
2. Feature-level: detect potential "red-lining" effects, i.e., are there other features or feature combinations that seem harmless but are highly correlated with the sensitive attribute on unfair samples?
3. Algorithm Self-Assessment: Does the bias attribution result align with expectations? Should the similarity measure be adjusted/replaced?

- ## Q3: Complexity of AIM

We wish to kindly refer you to Appendix B.2 - Complexity Analysis, where we provide a detailed time complexity analysis of AIM. In brief, bias/credibility estimation has a complexity of $\mathcal{O}(N^2)$, but both can be computed in matrix form, enabling fast parallel computation on modern GPUs (with $\mathcal{O}(\frac{N^2}{C})$ time complexity, where $C$ is the number of computing units and is normally large on modern GPUs). We placed the time complexity analysis in the appendix due to the 8-page limitation for the submission, and will relocate it to the main body of the paper in the 9-page camera-ready version.