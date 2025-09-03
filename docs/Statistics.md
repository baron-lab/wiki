---
layout: default
title: Statistics
parent: Useful Tools
nav_order: 6
---
# Statistics

## One or two groups
The t test is the most commonly used test to compare the means of two groups to determine if they are statistically different from each other. 
Types:
- One-sample t test can be used to determine if there is a difference between a group and a standard value (i.e., if a group mean is significantly different from zero)
- Paired (within-subject) t test is used to determine if there is a difference within a group at two points in time (i.e., before and after injury/intervention)
- Unpaired (between-subject) t test is used to determine whether there is a difference between groups (i.e., mice with different genotypes)

t tests assume the data is (1) independent, (2) normally distributed, and (3) have homogeneity of variance (similar amount of variance in each group). If data does not meet these assumptions, can use a non-parametric test such as the Wilcoxon Signed-Rank test. 

t tests can be performed in Matlab, R, excel, GraphPad Prism, and various others using premade functions.


## More than two groups

For more than two groups the Analysis of Variance (ANOVA) can be used to compare group means. 

There are a variety of types:
- One-way ANOVA: Tests the effect of one independent variable on the outcome measure. For example, testing whether MD is different between 3 groups (Control, AD, PD) at a single timepoint. 
- Two-way ANOVA: Tests the effect of two independent variables on the outcome measure. From the example above, can test whether MD if different between 3 groups (Control, AD, PD) at several timepoints (30, 50, 70 years of age). Two-way ANOVA also allows examination of interaction effects. In this example, the interaction effect could be used to examine group*time interactions, indicating whether there groups differ in their MD changes over time. 
- Analysis of Covariance (ANCOVA): Similar to ANOVA but adds covariate(s), a variable not of interest but could influence the outcome. In the example above, if you are only interested in examining group differences and not changes over time, time can be used as a covariate in an ANCOVA model to solely examine group differences. This works by regressing out the effect of the covariate from the outcome measure. 
- Repeated measure ANOVA: Can be used when the outcome measure are taken from the same subjects at multiple timepoints. Improves statistical power over a one-way ANOVA.

Similar to t tests, ANOVA tests assume data is (1) independent, (2) normally distributed, and (3) has homogeneity of variance. 


## Multiple comparison correction

In MRI research, it is common to compare groups across multiple brain regions (e.g., white matter, hippocampus) and MRI metrics (e.g., MD, FA, MK). However, increasing the number of statistical tests also increases the probability of false positives (Type I error). For instance, conducting 20 independent tests raises the chance of at least one false positive from 5% to ~64%. To address this, multiple comparison correction is required.

A widely used approach is the Bonferroni correction, which controls the family-wise error rate (FWER) by dividing the significance threshold by the number of tests. While effective at limiting false positives, Bonferroni is overly conservative in settings with many correlated tests, such as MRI, and can inflate false negatives (Type II errors).

An alternative is false discovery rate (FDR) correction, which ranks p-values and adjusts them based on their distribution. FDR allows a controlled proportion of false positives, providing greater sensitivity and power in large-scale, correlated comparisons.

In practice, Bonferroni is appropriate for a small number of independent tests (e.g., <5), whereas FDR correction is often preferred in MRI studies with many, correlated comparisons.


## Post-hoc testing

After a significant effect is detected from an ANOVA, the next step is to do post-hoc testing. If we detect a significant effect of group for MD between controls, AD, and PD groups, this means the groups are different overall, but doesn’t tell you exactly what groups are different. A post-hoc test can be used to determine which groups are significantly different from each other. The most common post-hoc test is Tukey’s HSD which tests all comparisons (controls vs AD, controls vs PD, AD vs PD). Post-hoc tests such as Tukey’s HSD, Bonferroni procedure, Fisher’s LSD are inherently multiple comparison corrected with varying degrees of stringency.


## Example in R 

df represents the data frame, which would have one column for the outcome measure (i.e., MD values) and factors for Group, Genotype, Time. 

``` bash
# Load CSV
df <- read.csv(paste0("data.csv"))

# Ensure factors
df$Group <- as.factor(df$Group)
df$Genotype <- as.factor(df$Genotype)
df$Time <- as.factor(df$Time)

# 3-way ANOVA for group, genotype, time effects with interactions (denoted by *)
model_3way <- aov(data ~ Group * Genotype * Time, data = df)

# Separate data from both genotypes to look for effects of group and time 
model_Gtype1 <- aov(data ~ Group * Time, data = filter(df, Genotype == 'Gtype1'))
model_Gtype2 <- aov(data ~ Group * Time, data = filter(df, Genotype == 'Gtype2’))
		
# Post-hoc: Group differences at each Timepoint in each genotype
Gtype1_emm <- emmeans(model_Gtype1, ~ Group | Time)
Gtype1_contrasts <- contrast(Gtype1_emm, method = "pairwise", adjust = "tukey")
Gtype2_emm <- emmeans(model_Gtype2, ~ Group | Time)
Gtype2_contrasts <- contrast(Gtype2_emm, method = "pairwise", adjust = "tukey")

# If you loop over multiple regions/metrics, can implement FDR correction
# ‘results’ contains columns with p-values from Group, Time, and Interaction effects from model_Gtype1     
# and model_Gtype2 for various regions/metrics
FDR_res$p_Group_fdr <- p.adjust(results$p_Group, method = "fdr")
FDR_res$p_Time_fdr <- p.adjust(results$p_Time, method = "fdr")
FDR_res$p_Interaction_fdr <- p.adjust(results$p_Interaction, method = "fdr")
```
