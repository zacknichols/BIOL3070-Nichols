Warm-up mini-Report: Mosquito Blood Hosts in Salt Lake City, Utah
================
Zack Nichols
2025-10-03

- [ABSTRACT](#abstract)
- [BACKGROUND](#background)
- [STUDY QUESTION and HYPOTHESIS](#study-question-and-hypothesis)
  - [Questions](#questions)
  - [Hypothesis](#hypothesis)
  - [Prediction](#prediction)
- [METHODS](#methods)
  - [Fill in 1st analysis
    e.g. barplots](#fill-in-1st-analysis-eg-barplots)
  - [Fill in 2nd analysis/plot e.g. generalized linear
    model](#fill-in-2nd-analysisplot-eg-generalized-linear-model)
- [DISCUSSION](#discussion)
  - [Interpretation of 1st analysis
    (e.g. barplots)](#interpretation-of-1st-analysis-eg-barplots)
  - [Interpretation of 2nd analysis (e.g. generalized linear
    model)](#interpretation-of-2nd-analysis-eg-generalized-linear-model)
- [CONCLUSION](#conclusion)
- [REFERENCES](#references)

# ABSTRACT

Understanding which avian hosts most strongly influences West Nile Virus
(WNV) transmission is essential for both predicting and mitigating the
effect of outbreaks. This study examines patterns of mosquito feeding
and its relationship to WNV-positivity presence across multiple sites in
the Salt Lake City area. Blood-fed mosquitoes were analyzed using PCR
and DNA sequencing procedures to identify vertebrate hosts, and this
data was compared with WNV testing results from mosquito pools. Host
population composition varied significantly between sites, with the
house finch and house sparrow blood meals occurring 4-5 times more often
at locations where WNV-positivity was detected. Generalized linear
models further revealed that the number of house finch blood meals
significantly predicted both the presence of WNV (*p* = 0.0287) and the
positivity rate (*p* = 0.0001). These results demonstrate that mosquito
feeding patterns on specific hosts, particularly house finches, are
strong indicators of viral amplification and could inform target public
health strategies.

# BACKGROUND

West Nile Virus (WNV) is a mosquito born virus of the family
*Flaviviridae* that cycles between mosquito and avian hosts, with humans
and mammals acting as a incidental, final host. Certain avian species,
such as the House Finch (*Haemorhous mexicanus*), are amplifying hosts
due to their ability to sustain high viremia levels (Komar et al.,
2003). Therefore, understanding which hosts mosquitoes feed on is
critical to identifying the both the ecological stressors and drivers of
WNV transmission. Blood meal analysis allows for host identification
through extracting DNA from the bio matter of blood-fed mosquitoes,
amplifying mitochondrial markers using PCR, and sequencing the products
for identification. When combined with WNV testing of mosquito pools,
this molecular approach could provide insight into how host use
correlates with viral presence. Thus, this information is essential for
predicting where and when WNV amplification is most likely to occur and
for informing targe public health interventions.

``` r
# Manually transcribe duration (mean, lo, hi) from the last table column
duration <- data.frame(
  Bird = c("Canada Goose","Mallard", 
           "American Kestrel","Northern Bobwhite",
           "Japanese Quail","Ring-necked Pheasant",
           "American Coot","Killdeer",
           "Ring-billed Gull","Mourning Dove",
           "Rock Dove","Monk Parakeet",
           "Budgerigar","Great Horned Owl",
           "Northern Flicker","Blue Jay",
           "Black-billed Magpie","American Crow",
           "Fish Crow","American Robin",
           "European Starling","Red-winged Blackbird",
           "Common Grackle","House Finch","House Sparrow"),
  mean = c(4.0,4.0,4.5,4.0,1.3,3.7,4.0,4.5,5.5,3.7,3.2,2.7,1.7,6.0,4.0,
           4.0,5.0,3.8,5.0,4.5,3.2,3.0,3.3,6.0,4.5),
  lo   = c(3,4,4,3,0,3,4,4,4,3,3,1,0,6,3,
           3,5,3,4,4,3,3,3,5,2),
  hi   = c(5,4,5,5,4,4,4,5,7,4,4,4,4,6,5,
           5,5,5,7,5,4,3,4,7,6)
)

# Choose some colors
cols <- c(rainbow(30)[c(10:29,1:5)])  # rainbow colors

# horizontal barplot
par(mar=c(5,12,2,2))  # wider left margin for names
bp <- barplot(duration$mean, horiz=TRUE, names.arg=duration$Bird,
              las=1, col=cols, xlab="Days of detectable viremia", xlim=c(0,7))

# add error bars
arrows(duration$lo, bp, duration$hi, bp,
       angle=90, code=3, length=0.05, col="black", xpd=TRUE)
```

<img src="Warm-up-mosquitoes-TEMPLATE--10.02.2025-_files/figure-gfm/viremia-1.png" style="display: block; margin: auto auto auto 0;" />

# STUDY QUESTION and HYPOTHESIS

## Questions

Which avian species contributes the most to the amplification of West
Nile Virus in Salt Lake City through their role as blood meal hosts for
mosquitoes?

## Hypothesis

House Finches are a key amplifying host of WNV in Salt Lake City due to
their ability to sustain high levels of viremia and their frequent
presence in mosquito blood meals.

## Prediction

If House Finches are important amplifying hosts, then mosquito pools
collected from locations where mosquitoes have fed on house finches
should have a higher rate of WNV positivity compared to pools from
locations with fewer or no House Finch blood meals.

# METHODS

Blood-fed mosquitoes were collected from multiple sites in the Salk Lake
City area, and blood meal biomass was extracted from their abdomens.
Using PCR, vertebrate mitochondrial markers were isolated and amplified.
THe resulting sequences were analyzed using BLAST to match each blood
meal to its most likely source.

## Fill in 1st analysis e.g. barplots

The number of mosquito blood meals from each host species at sites with
no WNV-positive mosquito pools was compared to sites with one or more
WNV-positive pools. This comparison helps to evaluate whether certain
host species are more frequently associated with cites where WNV is
actively circulating. A barplot is most useful for visualizing this
analysis, as it emphasizes differences in blood meal frequency across
groups, making patterns in host use and potential amplification easier
to interpret.

``` r
## import counts_matrix: data.frame with column 'loc_positives' (0/1) and host columns 'host_*'
#Insert ENTIRE working directory that leads to "bloodmeal_plusWNV_for_BIOL3070 in read.csv(). 
counts_matrix <- read.csv("bloodmeal_plusWNV_for_BIOL3070.csv")

## 1) Identify host columns
host_cols <- grep("^host_", names(counts_matrix), value = TRUE)

if (length(host_cols) == 0) {
  stop("No columns matching '^host_' were found in counts_matrix.")
}

## 2) Ensure loc_positives is present and has both levels 0 and 1 where possible
counts_matrix$loc_positives <- factor(counts_matrix$loc_positives, levels = c(0, 1))

## 3) Aggregate host counts by loc_positives
agg <- stats::aggregate(
  counts_matrix[, host_cols, drop = FALSE],
  by = list(loc_positives = counts_matrix$loc_positives),
  FUN = function(x) sum(as.numeric(x), na.rm = TRUE)
)

## make sure both rows exist; if one is missing, add a zero row
need_levels <- setdiff(levels(counts_matrix$loc_positives), as.character(agg$loc_positives))
if (length(need_levels)) {
  zero_row <- as.list(rep(0, length(host_cols)))
  names(zero_row) <- host_cols
  for (lv in need_levels) {
    agg <- rbind(agg, c(lv, zero_row))
  }
  ## restore proper type
  agg$loc_positives <- factor(agg$loc_positives, levels = c("0","1"))
  ## coerce numeric host cols (they may have become character after rbind)
  for (hc in host_cols) agg[[hc]] <- as.numeric(agg[[hc]])
  agg <- agg[order(agg$loc_positives), , drop = FALSE]
}

## 4) Decide species order (overall abundance, descending)
overall <- colSums(agg[, host_cols, drop = FALSE], na.rm = TRUE)
host_order <- names(sort(overall, decreasing = TRUE))
species_labels <- rev(sub("^host_", "", host_order))  # nicer labels

## 5) Build count vectors for each panel in the SAME order
counts0 <- rev(as.numeric(agg[agg$loc_positives == 0, host_order, drop = TRUE]))
counts1 <- rev(as.numeric(agg[agg$loc_positives == 1, host_order, drop = TRUE]))

## 6) Colors: reuse your existing 'cols' if it exists and is long enough; otherwise generate
if (exists("cols") && length(cols) >= length(host_order)) {
  species_colors <- setNames(cols[seq_along(host_order)], species_labels)
} else {
  species_colors <- setNames(rainbow(length(host_order) + 10)[seq_along(host_order)], species_labels)
}

## 7) Shared x-limit for comparability
xmax <- max(c(counts0, counts1), na.rm = TRUE)
xmax <- if (is.finite(xmax)) xmax else 1
xlim_use <- c(0, xmax * 1.08)

## 8) Plot: two horizontal barplots with identical order and colors
op <- par(mfrow = c(1, 2),
          mar = c(4, 12, 3, 2),  # big left margin for species names
          xaxs = "i")           # a bit tighter axis padding

## Panel A: No WNV detected (loc_positives = 0)
barplot(height = counts0,
        names.arg = species_labels, 
        cex.names = .5,
        cex.axis = .5,
        col = rev(unname(species_colors[species_labels])),
        horiz = TRUE,
        las = 1,
        xlab = "Bloodmeal counts",
        main = "Locations WNV (-)",
        xlim = xlim_use)

## Panel B: WNV detected (loc_positives = 1)
barplot(height = counts1,
        names.arg = species_labels, 
        cex.names = .5,
        cex.axis = .5,
        col = rev(unname(species_colors[species_labels])),
        horiz = TRUE,
        las = 1,
        xlab = "Bloodmeal counts",
        main = "Locations WNV (+)",
        xlim = xlim_use)
```

![](Warm-up-mosquitoes-TEMPLATE--10.02.2025-_files/figure-gfm/first-analysis-1.png)<!-- -->

``` r
par(op)

## Keep the colors mapping for reuse elsewhere
host_species_colors <- species_colors
```

## Fill in 2nd analysis/plot e.g. generalized linear model

A generalized linear model (GLM) was used to test whether the presence
or number of house finch blood meals predicted the likelihood of
detecting WNV-positive mosquito pools. This analysis allowed for a
formal evaluation of whether sites with more house finch feedings were
statistically more likely to have WNV present, or if higher feeding
frequency was associated with an increased WNV positivity rate. By
quantifying this relationship, the GLM provided a statistical framework
for assessing the influence of specific host species on amplification
dynamics.

``` r
# second-analysis-or-plot, glm with house finch alone against binary +/_
glm1 <- glm(loc_positives ~ host_House_finch,
            data = counts_matrix,
            family = binomial)
summary(glm1)
```

    ## 
    ## Call:
    ## glm(formula = loc_positives ~ host_House_finch, family = binomial, 
    ##     data = counts_matrix)
    ## 
    ## Coefficients:
    ##                  Estimate Std. Error z value Pr(>|z|)  
    ## (Intercept)       -0.1709     0.1053  -1.622   0.1047  
    ## host_House_finch   0.3468     0.1586   2.187   0.0287 *
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 546.67  on 394  degrees of freedom
    ## Residual deviance: 539.69  on 393  degrees of freedom
    ## AIC: 543.69
    ## 
    ## Number of Fisher Scoring iterations: 4

``` r
#glm with house-finch alone against positivity rate
glm2 <- glm(loc_rate ~ host_House_finch,
            data = counts_matrix)
summary(glm2)
```

    ## 
    ## Call:
    ## glm(formula = loc_rate ~ host_House_finch, data = counts_matrix)
    ## 
    ## Coefficients:
    ##                  Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)      0.054861   0.006755   8.122 6.07e-15 ***
    ## host_House_finch 0.027479   0.006662   4.125 4.54e-05 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for gaussian family taken to be 0.01689032)
    ## 
    ##     Null deviance: 6.8915  on 392  degrees of freedom
    ## Residual deviance: 6.6041  on 391  degrees of freedom
    ##   (2 observations deleted due to missingness)
    ## AIC: -484.56
    ## 
    ## Number of Fisher Scoring iterations: 2

# DISCUSSION

## Interpretation of 1st analysis (e.g. barplots)

The first analysis compared the host frequency of mosquito blood meals
between sites with no WNV-positive pools and those with at least one
WNV-positive pool. House finches (*Haemorhous mexicanus*) and house
sparrows (*Passer domesticus*) showed notably higher representation at
WNV-positive sites, with house finch blood meals occurring at almost 50
times at positive sites, compared to fewer than 10 times at negative
sites. Similarly, the house sparrow blood meals exceeded 45 detections
in positive locations, but were not noticeably frequent elsewhere. This
pattern indicates that mosquitoes feeding on these species are more
commonly associated with WNV presence, suggesting that they are key
amplifying hosts. The drastic difference in blood meal counts between
sites supports the idea that frequent feeding on these birds increases
the likelihood of further virus transmission.

## Interpretation of 2nd analysis (e.g. generalized linear model)

The generalized linear model (GLM) further tested whether house finch
blood meal frequency predicted WNV presence or positivity rate. The
binomial GLM yielded a significant positive effect of house finch blood
meals on WNV detection (Estimate = 0.3468 , p = 0.0287), which indicates
that each additional house finch blood meal increased the odds of
finding a positive mosquito pool. The second model testing WNV
positivity rate found an even stronger correlation (Estimate = 0.0275 ,
p \< .00001), supporting that sites with more house finch blood meals
tended to have substantially higher proportions of infected
mosquitoes.These results provide strong evidence that mosquito feeding
on house finches is a significant predictor of WNV amplification.

# CONCLUSION

Cumulatively, our analyses provide strong support for the hypothesis
that house finches are important amplifying hosts of WNV in the Salt
Lake City area. Mosquito blood meals from house finches were 4-5 times
more common at WNV-positive sites than at negative ones, and statistical
modeling confirmed that the blood meal frequency significantly predicted
both the presence and intensity of viral activity. From a perspective of
public health, these findings suggest that targeting surveillance and
control efforts in areas with abundant house finch populations could
improve early detection of WNV activity and help mitigate the effect of
future outbreaks.

# REFERENCES

1.  Komar N, Langevin S, Hinten S, Nemeth N, Edwards E, Hettler D, Davis
    B, Bowen R, Bunning M. Experimental infection of North American
    birds with the New York 1999 strain of West Nile virus. Emerg Infect
    Dis. 2003 Mar;9(3):311-22. <https://doi.org/10.3201/eid0903.020628>

2.  ChatGPT. OpenAI, version Jan 2025. Used as a reference for functions
    such as plot() and to correct syntax errors. Accessed 2025-10-03.
