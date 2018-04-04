# Weighted F-measure methodology based on relative cost and benefits

The standard methodology for measuring overall accuracy of a classification problem is to use the F-measure to determine optimal performance. Classification problems, such as a simple binary classification, require researchers to determine a cut-off point to determine how to classify any observation. A typical approach is to use the F-measure to determine the optimal cut-off score. The F-measure uses an unweighted harmonic mean of the precision and recall to calculate a score for each cut-off point. Researchers will typically use the maximum value of the F-measure to determine the appropriate cut-off.

However, real-world classification problems may not value precision and recall the same. For instance, a single false negative (FN) can be costly (e.g., failure to detect a faulty O-ring) so recall is highly valued. But, even if the ordinal importance is known, it is not known the specific weighting that should be used. This paper developes a framework which determines the relative weighting parameters that can be used in F-measure to re-weight the score to determine a new cut-off score.

Precision measures the ratio of true positives over all positives (true and false positives). Recall measures the ratio of true positives over true positives and false negatives. Thus, precision is sensitive to changes in false positive whereas recall changes with respect to false negative---all other terms are the same.

F-measure is defined as the harmonic mean between the two. Specifically,

```math
F = \dfrac{1}{ \frac{\alpha}{p} + \frac{(1 - \alpha)}{r} }
```
where $`\alpha`$ is used as the weight between precision and recall.


## Cost Function

Typically, classification problems are used to determine risk or prioritize queues. Positives (both true positives and false positives) will indicate some course of action should be taken. Negatives (both true negatives and false negatives) indicate no or delayed action. Thus, assuming that Positive cases carry some sort of costs (e.g., inspection cost) and that true positives (TP) have some net benefit (e.g., revenues from fines), we can determine the net cost of a Positive with:

```math
C_I \times N_I - C_p \times TP
```
where $`C_I`$ is the cost associated with a Positive case and $`N_I`$ is the number of positive cases (TP + NP). This term is the gross cost of Positives. Meanwhile, some benefit is recouped when the true positives (TP) yield a benefit, $`C_p`$.

Meanwhile, there are corresponding costs for doing nothing and mistakenly not doing work. We presume the cost of nothing is forgoing the benefits as true positives, that is, $`C_p \times FN`$. Putting the terms together, the total cost function is:

```math
K = \left( C_I \times N_I - C_p \times TP \right) + \left( C_p \times FN \right)
```

We can calculate the marginal change when false positives increase (precision decreases) as:

```math
\dfrac{\partial K}{\partial FP} = C_I
```

Likewise, increasing false negatives (reducing recall) will change the overall formula:

```math
\dfrac{\partial K}{\partial FN} = C_P
```

We can use the ratio of these terms to determine the relative cost of choosing either greater FP or FN as:

```math
\delta = \dfrac{C_I}{C_p}
```

The expected cost function is a function of the precision, $`p`$ and recall, $`r`$, rates for the model:

```math
E[K] = \left( C_I \times N_I - C_p \times p \right) + \left( C_p \times r \right)
```

## Optimal weights

To determine the final weights, we use the relative marginal cost $`\delta`$ to equate to the relative weight parameters from the F-measure. Specifically,

```math
\delta = \dfrac{\alpha}{1 - \alpha}
```

As $`\delta`$ increases, precision would be weighted less while recall would become more important in the F-measure. 

Regraphing the weight F-measure will yield different results than the weighted F-measure. Like before, researchers can use the maximum F-measure as a guide to set the cut-off score.