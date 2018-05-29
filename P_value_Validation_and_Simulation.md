P\_value Validation and Simulation
================
Evan YF W.
2018-05-28

Simulation is one way to verify p\_value
========================================

One question being asked previously is how we evaluate the efficacy of p-value, which means how do we further confirm the provided p-value is able to statistically reject or not reject one null hypothesis - Here I suggest way, Simulation and demo a simple case to demostrate how this approach could verify p\_value. \# t-test with equal sample sizes

``` r
rep <- 1e4
mu1 <- 5
mu2 <- 5
sigma <- 2
n1 <- 10
n2 <- 10
tval <- numeric(rep)
pval <- numeric(rep)

for(i in 1 :rep){
  x1 <- rnorm(n1, mu1, sigma)
  x2 <- rnorm(n2, mu2, sigma)
  S <- sqrt(((n1 - 1) * var(x1) + (n2 - 1) * var(x2)) / (n1 + n2 - 2))
  tval[i] <- (mean(x1) - mean(x2)) / (S * sqrt(1 / n1 + 1 / n2))
  pval[i] <- 2 * pt(abs(tval[i]), df = min(n1, n2) - 1, lower = FALSE)
}
hist(pval, freq = FALSE, breaks = seq(0, 1, 0.05), col = c("red", rep("white", 19)),
     main = "Distribution of p-values\nfor t-test with equal sample size")
```

![](P_value_Validation_and_Simulation_files/figure-markdown_github/unnamed-chunk-1-1.png)

t-test with unequal sample sizes
================================

Here we simulate the results of t-test under the condition with 2 unequal sample sizes, and we would like to see how it could affect the performance of p-value - From the follwing plot about the distribution of simulated p-values for t-test with this case, one can find that given the p-value less or equal than 0.05, the probability would be roughly around 0.005(0.1/20). Based on the ideal scenario, if a null hypothesis is being not rejected, the probability of p-value &lt;= 0.05 should be also around 0.05. In other words, the probability of p-value &lt;= 0.05 is the one of Type I error(Null hypothesis is correct but being rejected). Thus, our results showed this unequal sample size approach does lower the chance of Type I error, at the same time, increase the chance of Type II error theorically(cause alternative hypothesis is being verified, we tend to fail to reject hull hypothesis)

``` r
rep <- 1e4
mu1 <- 5
mu2 <- 5
sigma <- 2
n1 <- 5
n2 <- 50
tval <- numeric(rep)
pval <- numeric(rep)

for(i in 1 :rep){
  x1 <- rnorm(n1, mu1, sigma)
  x2 <- rnorm(n2, mu2, sigma)
  S <- sqrt(((n1 - 1) * var(x1) + (n2 - 1) * var(x2)) / (n1 + n2 - 2))
  tval[i] <- (mean(x1) - mean(x2)) / (S * sqrt(1 / n1 + 1 / n2))
  pval[i] <- 2 * pt(abs(tval[i]), df = min(n1, n2) - 1, lower = FALSE)
}
hist(pval, freq = FALSE, breaks = seq(0, 1, 0.05), col = c("red", rep("white", 19)),
     main = "Distribution of p-values\nfor t-test with unequal sample size")
```

![](P_value_Validation_and_Simulation_files/figure-markdown_github/unnamed-chunk-2-1.png)