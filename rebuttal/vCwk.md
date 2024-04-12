**Thank you for viewing the detailed response we prepared for you! Your opinion is important to us to further improve the paper, and we are more than happy to address any further questions/concerns you may have. If our efforts have resolved your concerns, we sincerely hope and greatly appreciate your consideration in supporting the acceptance of our work.**

**We provide detailed responses to each of your questions below.**

- ## Q1: Regarding the theoretical underpinnings of AIM_AUG

We would like to address your concern by further elaborating on (1) the connection between AIM_AUG and AIM_REM, (2) the efficacy of AIM_AUG from both utility and fairness perspectives, and (3) the theoretical implication of AIM bias attribution.

### Connection between AIM_AUG and AIM_REM
Essentially, AIM_AUG serves as a "soft" version of AIM_REM: while AIM_REM achieves debiasing by directly removing high-bias samples from the data, AIM_AUG reduces the importance of high-bias samples in the data by increasing the frequency of low-bias samples. Both can be seen as reducing the weight of high-bias samples in the training data, but AIM_REM achieves this through hard downsampling (i.e., downweighting high-bias samples to 0), while AIM_AUG does so by implicitly reducing the weight of high-bias samples through oversampling low-bias ones.

### The efficacy of AIM_AUG from a utility and fairness perspective

- **Fairness-wise:** As discussed above, AIM_AUG operates as a "soft" version of AIM_REM, and both debias the data in a similar manner by downweighting high-bias samples. Furthermore, we emphasize that AIM_AUG is also well-aligned with the individual fairness principle: through neighborhood mixup, AIM_AUG introduces synthetic samples with the same labels into the neighborhood of existing samples, ensuring that the model produces similar outputs for similar samples in the neighborhood, which aligns with the objective of individual fairness that "similar individuals should also be treated similarly." Thus, AIM_AUG can help promote individual fairness while reducing the weight of high-bias samples through augmentation (as shown in Table 1).
- **Utility-wise:** Unlike AIM_REM that discards information from the original data, AIM_AUG retains and utilizes all real-world information in the large training set, leading to better generalizability and classification utility (as demonstrated in Figure 2 and Table 1).

### Theoretical underpinning of AIM

Finally, we wish to highlight the theoretical implications of the sample-level bias addressed in our work and compare it with existing notions of group/individual fairness. This can intuitively explain why AIM_AUG/REM can alleviate both group-level and individual-level biases present in the data.

- **Group fairness (GF):** The typical form of GF requires a consistent statistic across different groups. Take demographic parity an example, the objective is $P(Y| S=s_i) = P(Y|S=s_j)$, where $S$ denotes sensitive membership. It's evident that GF's objective is independent of individual features $X$ and does not regulate the outcome at individual-level.
- **Individual fairness (IF):** Conversely, IF only demands similar individuals to receive similar outcomes, simplified as $P(Y| X=x_i) \approx P(Y| X=x_j), \forall x_i \approx x_j$ (in practice, IF requires the Lipschitz property between model inputs and outputs). This objective is independent of sensitive membership $S$ and does not consider the fairness between groups.
- **AIM:** By comparison, our sample bias objective can be practically formulated as $P(Y| X=x_i, S=s_i) \approx P(Y| X=x_j, S=s_j), \forall x_i \approx x_j, \forall s_i, s_j$. This simultaneously requires fairness at both individual level (as we require consistency near a specific point $x_i$) and group level (as it necessitates alignment between different groups).

Therefore, AIM's bias definition captures both group-level and individual-level bias. As discussed above, both AIM_AUG/REM reduce the importance of high-bias (violating group and/or individual fairness) samples in the training data, thereby effectively debiasing the original data and lead to more equitable model outcomes.

- ## Q2: Regarding complexity analysis

Thank you for your careful reading of our paper. We will relocate the complexity analsysis to the main body of the paper in the 9-page camera-ready version. It was placed in the appendix due to the 8-page limitation for the submission.