**Thank you for viewing the detailed response we prepared for you! Your opinion is important to us to further improve the paper, and we are more than happy to address any further questions/concerns you may have. If our efforts have resolved your concerns, we sincerely hope and greatly appreciate your consideration in supporting the acceptance of our work.**

**We provide detailed responses to each of your questions below.**

- [Q1: Discussion on the sample comparability and similarity](#q1-discussion-on-the-sample-comparability-and-similarity)
  - [How were these values chosen for the experiments?](#how-were-these-values-chosen-for-the-experiments)
  - [Further analysis on the impact of different values](#further-analysis-on-the-impact-of-different-values)
- [Q2: Can methods devised for group fairness lead to individual unfairness?](#q2-can-methods-devised-for-group-fairness-lead-to-individual-unfairness)
- [Q3: Regarding case study](#q3-regarding-case-study)
- [W1: Clarifications regarding individual and group fairness](#w1-clarifications-regarding-individual-and-group-fairness)
- [W2: Remarks on Figure 2 (group fairness) and Table 1 (individual fairness)](#w2-remarks-on-figure-2-group-fairness-and-table-1-individual-fairness)
- [W3: Related works](#w3-related-works)

## Q1: Discussion on the sample comparability and similarity

We kindly refer you to Appendix A.2 - Hyperparameters, where we discuss how we select sample comparability parameters and their practical implications. In essence, the numerical ($t_r$) and categorical ($t_d$) disparity thresholds used in sample comparability determine the locality/strictness of similarity: smaller values yield more localized similarity. 

### How were these values chosen for the experiments?

Intuitively, a large $t_r$/$t_d$ can fail to capture semantic similarity adequately due to weak comparability constraints. Conversely, overly small $t_r$/$t_d$ values may result in too few comparable sample pairs, leaving more samples with no available comparable samples (we cannot perform bias attribution on such samples because of lack of evidence). During experimentation, we evaluated various values for both $t_r$ (ranging from 0.01 to 0.2) and $t_d$ (ranging from 1 to 5), ultimately choosing $t_r=0.1$ and $t_d=2$. These values are large enough to ensure that over 95% of samples have at least one comparable sample, while also striking a balance to maintain reasonable semantic similarity between comparable samples.

### Further analysis on the impact of different values

To visually demonstrate the impact of different disparity thresholds on AIM, we have conducted a series of synthetic experiments to illustrate how various values of $t_r$ affect AIM bias attribution. The results are depicted in the following figure. Each row in the figure represents a synthetic dataset, with the bias type of the data labeled at the beginning of each row. For each dataset, the first and second columns display the data and class distribution of groups #1 and #2, respectively. The third column shows the ground truth bias distribution on the target group (#2), where blue indicates unbiased and red indicates biased. Columns 4-9 display the distribution of bias scores obtained using different $t_r$ values (annotated in the caption) with AIM. 

![](https://github.com/AnonAuthorAI/AIM/blob/main/figs/synthetic_parameter_analysis.png?raw=true)

To summarize:

- AIM is not sensitive to the disparity threshold; as long as $t_r$ is within a reasonable range (0.1-0.3), AIM can accurately attribute bias, closely aligning with the ground truth bias (third column).
- However, using extremely small $t_r$ values (e.g., $t_r=0$: requiring complete feature match for comparability) results in most samples lacking available comparable samples, hindering AIM's ability to perform bias attribution under such circumstances (though AIM can still function normally if a large portion of the data consists of samples with identical features, depending on the specific application scenario).
- Conversely, using excessively large $t_r$ values (e.g., $t_r=0.9$) leads to almost every sample being comparable to nearly all samples in the dataset, resulting in overly smoothed bias scores that fail to provide effective information.

Finally, we emphasize that extreme values of disparity thresholds, such as $0$ and $0.9$, correspond to extremely strong/weak comparability constraints, which are rarely used in practical settings. In pracitce, AIM can provide reasonable bias attribution as long as users input values within a reasonable range, as demonstrated in the experiments above.


## Q2: Can methods devised for group fairness lead to individual unfairness?

Yes, FairML methods designed for group fairness (GF) can also potentially (but not necessarily) lead to individual unfairness. For deeper insights into possible conflicts between GF and IF and how to mitigate them, please kindly refer to the paper "On the apparent conflict between individual and group fairness." [1] In short, GF and IF may conflict in cases where, as a result of trying to satisfy group fairness, pairs of individuals who are otherwise similar but differ in a protected characteristic are assigned different outcomes. However, this conflict is more of an artefact of the blunt application of fairness measures, rather than a matter of conflicting principles [1]. In other words, whether GF methods lead to individual unfairness in practice depends on the nature of the GF methods themselves and the dataset. 

To further illustrate this point, we provide additional experimental results on compas dataset in the table below (excluding Reduction since it does not accept sensitive attributes as input during inference, thus Prediction Consistency cannot be tested). It can be observed that different GF methods exhibit varying impacts on individual fairness (IF). For gender task, AdaFair tends to induce individual unfairness, while other methods like LearnFairRep can mitigate IF to some extent. However, whether GF methods lead to individual unfairness also depends on the nature of the task: in race tasks, most fairness-aware methods result in individual unfairness. This is aligned with the discussion in [1]. In practice, the potential conflict between GF and IF is a complex issue, contingent on the properties of algorithm, data, and the GF/IF metric adopted.

![](https://github.com/AnonAuthorAI/AIM/blob/main/figs/GF-IF.png?raw=true)

> References:  
> [1] Binns, Reuben. "On the apparent conflict between individual and group fairness." Proceedings of the 2020 conference on fairness, accountability, and transparency. 2020.  

## Q3: Regarding case study

Thank you for this great point. Firstly, we want to note that the workclass "Local-gov" actually has a higher proportion of high-income individuals (positive label) at 29.52% compared to 21.77% in "Private" in the Adult dataset. In other words, **the reason why example #1 has a low income compared to her comparison/explanation points is not because her job "Local-gov" typically has lower income.** Therefore, we consider this example to reflect unfairness.

Additionally, we would like to clarify that the labels in Table 2 are not predictions but the acutal ground-truth labels observed from the real world. Thus the example #1 may also imply that women working in "Local-gov" have lower incomes compared to men in the real-world. Your comment has inspired us to further investigate the data and we found that only 17.93% of females working in "Local-gov" have high incomes, far lower than the 37.28% for males. While this may not strictly prove gender-based pay disparities, it does illustrate an important value of AIM: assisting human experts in discovering patterns between features and sensitive attributes in reviewing high-bias data. This can further aid in fairness-aware feature engineering, such as the "measure the relative importance of the different features" as you suggested: upon discovering discrimination linked to the feature "workclass=Local-gov," human experts can adjust its importance accordingly during training. Furthermore, extending AIM to automatically discover feature importance is also an intriguing future direction. We will include more discussion on this in the camera-ready version.

## W1: Clarifications regarding individual and group fairness

We wish to answer your question by highlighting the difference between the fairness objectives of GF, IF, and AIM. 

- **Group fairness (GF):** The typical form of GF requires a consistent statistic across different groups. Take demographic parity an example, the objective is $P(Y| S=s_i) = P(Y|S=s_j)$, where $S$ denotes sensitive membership. It's evident that GF's objective is independent of individual features $X$ and does not regulate the outcome at individual-level.
- **Individual fairness (IF):** Conversely, IF only demands similar individuals to receive similar outcomes, simplified as $P(Y| X=x_i) \approx P(Y| X=x_j), \forall x_i \approx x_j$ (in practice, IF requires the Lipschitz property between model inputs and outputs). This objective is independent of sensitive membership $S$ and does not consider the fairness between groups.
- **AIM:** By comparison, our sample bias objective can be practically formulated as $P(Y| X=x_i, S=s_i) \approx P(Y| X=x_j, S=s_j), \forall x_i \approx x_j, \forall s_i, s_j$. This simultaneously requires fairness at both individual level (as we require consistency near a specific point $x_i$) and group level (as it necessitates alignment between different groups).

We'll include relevant discussions in the camera-ready version.

## W2: Remarks on Figure 2 (group fairness) and Table 1 (individual fairness)

We would like to clarify that Table 1 is not a reorganization or tabular presentation of Figure 2. 
They are independent experimental results, comparing different sets of baseline methods with different settings. Specifically, the baselines in Figure 2 are designed for group fairness (GF), while the methods compared in Table 1 are designed for individual fairness (IF).

We chose to report results on the Adult dataset in Table 1 to ensure a fair comparison: the baseline methods SenSR and SenSEI are only implemented on the Adult dataset in their respective official code repositories (their papers also only reported results on the Adult dataset). Their data preprocessing, hyperparameters, and model architectures are tightly coupled with this dataset, thus cannot be directly applied to new data. Even if extensive unofficial engineering efforts were made to run them, we cannot guarantee optimal performance of this baselilne. For these reasons, we followed the official implementation and parameters given in the original paper to ensure a fair comparison.

## W3: Related works

Thank you for your careful review of our paper. We believe it is crucial to state the connections between AIM and existing fairness notions when introducing the definitions, so we have presented the detailed discussion to Section 3.1. Due to the 8-page limit for submission, we can only provide a brief summary of existing fairness concepts in the related work section. In the 9-page camera-ready version, we will include more detailed discussions for each type of fairness, including popular definitions and representative FairML techniques.