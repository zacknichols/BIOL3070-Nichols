Pruning Effects on Apple Tree Harvest
================
Zack Nichols
2025-12-05

- [ABSTRACT](#abstract)
- [BACKGROUND](#background)
- [STUDY QUESTION and HYPOTHESIS](#study-question-and-hypothesis)
  - [Questions](#questions)
  - [Hypothesis](#hypothesis)
  - [Prediction](#prediction)
- [METHODS](#methods)
  - [Statistical Analysis](#statistical-analysis)
  - [Data Visualization](#data-visualization)
- [DISCUSSION](#discussion)
  - [Interpretation of Statistical
    Analysis](#interpretation-of-statistical-analysis)
  - [Interpretation of Data
    Visualization](#interpretation-of-data-visualization)
- [CONCLUSION](#conclusion)
- [REFERENCES](#references)

# ABSTRACT

Pruning is a widely used orchard management practice in perennial plant
species to shape canopy structures, improve light penetration and
distribution, and influence overall patterns and abundance of fruit
production. This study evaluated whether pruning practice was related to
differences in harvest-yield rating among apple trees in from the
community research effort *The Wasatch Back Fruit Tree Project*. Each
tree was categorized as pruned or unpruned, and assigned a yield rating
on a three-level ordinal scale (Minimal, Normal, Above Average).
Statistical analysis of the data showed no significant relationship
between either pruning treatment and yield classification (*χ²* = 2.53,
*df* = 2, *p* = 0.28). Additionally, our data visualization further
illustrated substantial overlap in yield score distributions between
groups. These findings indicate that pruning practice did not produce
any significant differences in the categorical yield outcomes that were
available in the dataset. Several factors may explain this lack of
pattern. The timing of pruning can strongly influence its effectiveness,
often stunting its positive qualities if done too early. Additionally,
the data collection used in the communal study lacked standardized
definition, which may have led to unintentional bias and statistical
noise that effected our results.

# BACKGROUND

Successful apple production depends on a variety of factors, from
environmental conditions to orchard management practices. One of these
management practices, pruning, is recognized as one of the most
essential techniques for maintaining tree productivity. Similar to tree
species that do *not* produce fruit, apple trees naturally invest energy
into vegetative growth, often producing dense canopies that limit
internal light ability. This reduced light penetration can inhibit
flower bud formation, restrict fruit development, and ultimately lower
the overall harvest quality from the tree (Gomasta et al., 2024).
Pruning is used to open the canopy, improve sunlight distribution across
all leaves, and encourage the production of fruiting wood, making the
practice a crucial component of orchard management

Beyond simply improving light exposure, pruning also influences other
factors supplemental to fruit growth. These include air flow, disease
pressure, plant stability, and growth distribution. Strategically
removing crowded or shaded branches helps a tree to maintain vigor,
while supporting consistent fruit production across seasons, and also
increase the quality and development of the individual fruits. As a
result, pruned trees are expected to achieve more reliable yields than
unpruned plants, especially in the case of environments where
environmental variability can compound stress on fruit development
(Renquist, 2018).

In many commercial and research-oriented orchards, harvest success is
recorded using categorical classifications when direct, quantitative
counts of fruit number or biomass are unavailable. These categories
provide a practical means of assessing overall harvest trends across
large populations of trees, without comparing them to one another.
Understanding whether pruning increases the likelihood of a higher
harvest classification is important for optimizing orchard efficiency,
participatory in regions like Northern Utah where unpredictable
fluctuations in temperature, precipitations, and pollinator activity can
affect fruit yield.

This study examines whether pruned apple trees are more likely to
receive higher categorical harvest ratings compared to unpruned trees,
providing insight into the role of pruning practices in determining
yield outcomes.

# STUDY QUESTION and HYPOTHESIS

## Questions

How does annual pruning influence the likelihood of achieving higher
categorical harvest ratings among apple trees in Northern Utah orchards?

## Hypothesis

If pruning enhances canopy light penetration and resource allocation
within apple trees, then pruned trees will produce a significantly
higher fruit average harvest rating compared to unpruned individuals.

## Prediction

If our hypothesis is correct, we expect to see that pruned trees have a
higher proportion of *Above Average* harvest yield classifications, and
a lower proportion of *Minimal* yields relative to unpruned trees,
indicating a positive association between pruning and categorical
harvest outcomes.

# METHODS

The data set used in this study was obtained from the Wasatch Back Fruit
Tree Project, which is a citizen-led research initiative coordinated
through Utah State University’s Extension. The project compiled
information on fruit tree varieties that reliably produce along the
Wasatch Front (Utah State University, 2024). The dataset contained
information on individual apple trees, including their pruning status
and a categorical harvest yield rating. Each tree was labeled with a
unique identification tag, assigned a pruning treatment (“Yes” or “No”),
and given a yield score of either Minimal, Normal, or Above Average. For
the purposes of our study, we converted these scores to numerical values
(1 = minimal yield, 2 = normal yield, 3 = high yield).

The dataset was examined before statistical analysis, and all missing or
incorrectly formatted entries were removed. Pruning status was converted
from a binary variable to a factor variable, and yield scores were
treated as ordinal categorical values. This cleaned data served as the
basis for evaluating whether pruning practice was associated with
differences in harvest-yield classifications.

## Statistical Analysis

To evaluate whether pruning treatment influenced the distribution of
harvest rating among apple trees, a chi-square test of independence was
used. Due to harvest yield was recorded in this data set using three
ordered categories (Minimal, Normal, and Above-Average), this analysis
provides a formal assessment of whether the frequency with which they
received each rating differs when the tree is pruned or unpruned. By
comparing observed category counts to the frequencies expected in the
event of no association, the chi-squared tests offers a statistical
framework for determining whether pruning practice was related to shift
in overall harvest classification.

``` r
# Chi Squared Test
tbl <- table(pruning_data$pruningstatus,
             pruning_data$sizeofharvest)

tbl
```

    ##           
    ##             1  2  3
    ##   Pruned   40 38  4
    ##   Unpruned 22 34  5

``` r
chi_result <- chisq.test(tbl,
                         correct = FALSE)

chi_result
```

    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  tbl
    ## X-squared = 2.5298, df = 2, p-value = 0.2823

## Data Visualization

To generate a visualization of the relationship between pruning
treatment and harvest yield categories, a stacked bar chart was created.
This plot shows the proportional contribution of each categorical yield
classification within both pruned and unpruned populations. This allows
for clear comparison of how frequently *Minimal*, *Normal*, and
*Above-Average* yield grades occurred among each group. Since in our
data set, the response variable is ordinal and categorical, this stacked
bar chart provides an accurate representation of relative differences in
the frequency of each category, which is complimented by the statistical
analysis our chi-squared test provides.

``` r
# Stacked bar chart 
library(scales)

harvest_colors <- c(
  "Minimal" = "#D4B86A",
  "Normal" = "#97C29A",
  "Above-Average" = "#8CB7D6"
)

ggplot(pruning_data_clean, aes(x = pruningstatus, fill = sizeofharvest)) +
  geom_bar(position = "fill") +
  scale_fill_manual(values = harvest_colors) +
  labs(
    x = "Pruning Status",
    y = "Proportion of Trees",
    fill = "Harvest Category",
    title = "Distribution of Harvest Grades by Pruning Treatment"
  ) +
  scale_y_continuous(labels = percent_format()) +
  theme_minimal(base_size = 15)
```

![](Final-Project-Nichols_files/figure-gfm/second-analysis-1.png)<!-- -->

# DISCUSSION

## Interpretation of Statistical Analysis

The chi-square test evaluated whether pruning treatment had an influence
on the distribution of categorical harvest-yield ratings. After
reviewing the results, the test indicated no significant association
between pruning and harvest classification (*χ²* = 2.53, *df* = 2, *p* =
0.28). Although pruned and unpruned trees differed slightly in the
frequencies of “Minimal”, “Normal”, and “Above Average” yield scores,
these differences were not statistically significant. These results
suggest that pruning did not measurably shift the likelihood of a tree
receiving a higher yield rating in this dataset.

## Interpretation of Data Visualization

The stacked bar chart illustrates the proportional distribution of
harvest-yield grades across both pruned and unpruned tree populations.
Both treatments exhibit extremely similar patterns, with *Normal* yields
comprising the largest proportions of observations, followed by
*Minimal* yields, and a small fraction of *Above-Average* yields.
Unpruned trees visually show a slightly higher proportion of *Minimal*
yields and pruned trees show a marginally greater proportion of *Normal*
yields; however these difference are not significant enough to indicate
a meaningful shift in yield category frequencies. The substantial
overlap in the yield outcomes between pruning treatments aligns with the
chi-squared analysis results, reinforcing that pruning practice did not
produce a clear difference in the likelihood of a tree receiving a
higher yield rating.

# CONCLUSION

This study found no significant association between either pruning
treatment and categorical harvest ratings in apple trees. Although
pruning is a orchard management practice that is generally expected to
improve canopy structure, light penetration and distribution, and
fruiting potential, these effects were not reflected in the yield
classifications observed in this case. One possible explanation for
these results is that the effectiveness of all orchard management
strategies, pruning included, are extremely dependent on *when* they
take place. If limbs are cut prematurely, pruning is shown to to
actually suppress shoot development rather than encourage it, reducing a
tree’s capacity to allocate resources toward fruit production, thus
stunting the intended positive effects (Lott et al., 2018). Because the
dataset only mentioned whether or not it pruning been done and lacked
information on specific timing and severity of that pruning, variation
may have contributed to the absence of yield differences that we had
expected to see. In addition, the use of broad, categorical yield
classifications rather than continuous, quantitative measurements may
have reduced the precision of the analysis. Yield ratings lacked
standardized definitions across sites, and different observers may have
applied these classifications inconsistently. This would introduce noise
that could obscure true differences between pruned and unpruned trees
(Utah State University, 2024). In future studies, it would be beneficial
to integrate detailed pruning records, sampling over several consecutive
years, and continuous yield measurements rather than categorical
variables. This could provide a more complete understanding of how
pruning and other orchard management techniques might influence fruit
production.

# REFERENCES

1.  Gomasta, J., Sarker, B., Haque, M., Anwari, A., Mondal, S., &
    Uddin, S. (2024). Pruning techniques affect flowering, fruiting,
    yield and fruit biochemical traits in guava under transitory
    sub-tropical conditions. Heliyon, e30064–e30064.
    <https://doi.org/10.1016/j.heliyon.2024.e30064>

2.  Renquist, S. (2018, July 3). Fruit thinning. OSU Extension Service.
    <https://extension.oregonstate.edu/catalog/em-9598-fruit-thinning>

3.  Utah State University. (2024). About the Wasatch Back Fruit Tree
    Project. Usu.edu. <https://extension.usu.edu/wasatch/WBFTP_about>

4.  Lott, D., Fisk, C., & Hammond, V. (2018). Pruning Fruit Trees.
    University of Nebraska.
    <https://extensionpubs.unl.edu/publication/ec1233/2018/pdf/view/ec1233-2018.pdf>

5.  ChatGPT. OpenAI, version Jan 2025. Used as a reference for functions
    such as plot() and to correct syntax errors. Accessed 2025-12-05.
