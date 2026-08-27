# TMB computer exercise: Production with unobserved firm productivity

## Economic setting

A panel of firms produces output according to

\[
y_{it} = \alpha + \beta k_{it} + a_i + \varepsilon_{it},
\]

where:

- \(y_{it}\) is log output,
- \(k_{it}\) is log capital,
- \(a_i\) is time-invariant unobserved firm productivity,
- \(\varepsilon_{it}\) is an idiosyncratic shock.

Assume

\[
a_i \sim N(0, \sigma_a^2), \qquad
\varepsilon_{it} \sim N(0, \sigma_\varepsilon^2).
\]

TMB treats the firm effects \(a_i\) as random effects and integrates them out using the Laplace approximation.

## Learning goals

Participants will:

1. translate a simple economic model into a joint negative log-likelihood;
2. use positive scale parameters through log transformations;
3. compile and fit a TMB model in R;
4. interpret the production elasticity and productivity dispersion;
5. compare a pooled OLS estimate with the random-effects TMB estimate.

## Files

- `simulate_data.R`: simulates a reproducible firm panel.
- `production_starter.cpp`: participant template containing TODOs.
- `run_exercise.R`: compiles and estimates the participant model.
- `production_solution.cpp`: complete solution.
- `run_solution.R`: runs the complete solution.

## Software required

In R, install TMB once:

```r
install.packages("TMB")
```

A working C++ compiler is also required. On Windows, install the version of Rtools that matches your R version. On macOS, install the Xcode command-line tools.

## Participant tasks

### Task 1: Complete the TMB likelihood

Open `production_starter.cpp` and replace each TODO. The joint negative log-likelihood is

\[
-\sum_i \log f(a_i) - \sum_{i,t}\log f(y_{it}\mid a_i).
\]

Hints:

- transform `log_sigma_a` and `log_sigma_eps` with `exp()`;
- use `dnorm(value, mean, sd, true)`;
- observation `n` belongs to firm `firm(n)`;
- the conditional mean is `alpha + beta * capital(n) + a(firm(n))`.

### Task 2: Estimate the model

Run:

```r
source("simulate_data.R")
source("run_exercise.R")
```

Record estimates of \(\alpha\), \(\beta\), \(\sigma_a\), and \(\sigma_\varepsilon\).

### Task 3: Economic interpretation

Answer:

1. In a log–log specification, how do you interpret \(\beta\)?
2. What does a large \(\sigma_a\) imply about firms?
3. Why would pooled OLS understate uncertainty when repeated observations from the same firm share \(a_i\)?
4. Compare the TMB estimate of \(\beta\) with pooled OLS.
5. What identifying assumption is needed for a random-effects interpretation of \(\beta\)?

### Optional extension

Modify the data-generating process so that productivity is correlated with capital. Investigate how this affects pooled OLS and the random-effects estimate. Then discuss whether a fixed-effects approach is preferable.

## Expected benchmark

The simulation uses \(\alpha=1\), \(\beta=0.6\), \(\sigma_a=0.5\), and \(\sigma_\varepsilon=0.25\). Estimates will not equal these values exactly because the sample is random.
