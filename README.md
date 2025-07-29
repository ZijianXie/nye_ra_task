
# README: Task 2 – Structural Model Estimation of Consumer Search

Below is a description of each file and its purpose in the estimation process.

---
## 1. `B.m` – **Helper Function for Reservation Utility**

### Reservation Utility Formula

This function computes the reservation utility value using the Weitzman-style search model:

```matlab
function y = B(m)
y = (1 - normcdf(m)) * ((normpdf(m) / (1 - normcdf(m))) - m);
end
```

Mathematically, this is:

$$
B(m) = (1 - \Phi(m)) \cdot \left( \frac{\phi(m)}{1 - \Phi(m)} - m \right)
$$

Where:
- $\Phi(m)$ is the cumulative distribution function (CDF) of the standard normal distribution.
- $\phi(m)$ is the probability density function (PDF) of the standard normal distribution.



Used to invert the reservation utility equation, based on normal distribution PDF and CDF.
## 2. `estWeitzD100W10S1.m` – Main Estimation Script

This is the main driver script used to simulate data, estimate parameters, and save results for the structural consumer search model.

### Key Steps:

- **Reads configuration**:
  - `D`: Number of epsilon and eta draws for simulation.
  - `W`: Scaling parameter for the softmax decision rule.
  - `S`: Random seed for reproducibility.

- **Data simulation**:
  - Calls `simWeitz()` with the input parameters to simulate consumer search and choice behavior.
  - Saves the simulated dataset into a `.mat` file.

- **Likelihood estimation**:
  - Loads the simulated data.
  - Generates `D` draws of random utility shocks from the standard normal distribution.
  - Calls the function `liklWeitz1()` to evaluate the likelihood given parameters and data.
  - Uses `fminunc()` (MATLAB’s optimization solver) to find parameters that maximize the likelihood.

- **Outputs**:
  - Saves estimated parameters and metadata into:
    - 'rezSimWeitzD%dW%dS%d.csv': Estimated coefficients and likelihood values.
    
---

## 3. `liklWeitz1.m` – Outer Likelihood Function

This script calculates the **average likelihood across consumers** for the structural search model.

### Key Steps:
- **Loops through all consumers** and retrieves relevant rows for each.
- For each consumer:
  - Extracts their data across alternatives.
  - Draws `D` sets of utility shocks:
    - `epsilonDraw`: standard normal draw for idiosyncratic utility shock.
    - `etaDraw`: standard normal draw for pre-search utility noise.
  - Calls `liklWeitz2()` to compute the consumer-level likelihood.

- **Returns**: the **negative mean log-likelihood** across all consumers.
- **Used by**: `fminunc()` inside `estWeitzD100W10S1.m` to perform MLE.

---

## 4. `liklWeitz2.m` – Inner Likelihood Function (Core Search Logic)

This function calculates the **probability of observed behavior** (search and choice) under the Weitzman-style structural model.

### Core Computation:

- Computes expected utility:
  ```matlab
  eut=repmat(xb,1,D)+etaDraw.*(1-outside);
  ut = eut + epsilonDraw;
  ```

- Loads `tableZ.csv` and inverts the function:
  $$
  c = B(m) \Rightarrow m = B^{-1}(c)
  $$
  using a lookup table to solve for reservation utility \( z = m + eut \).

- Loads `tableZ.csv` and inverts the function: `c = B(m) → m = B⁻¹(c)` using a lookup table to solve for reservation utility (`z = m + eut`).

- Implements three decision rules:

  1. **Selection Rule** – choosing which product to search:  
     `denom_order = exp(scaling * (z - z_max))`

  2. **Stopping Rule** – deciding when to stop searching:  
     `denom_search = exp(scaling * (z - y_max))`

  3. **Choice Rule** – choosing among searched alternatives:  
     `denom_ch = exp(scaling * (u* - u))`


- **Final likelihood**:
  - Inverts all decision denominators into probabilities.
  - Averages across `D` simulated draws.
  - Returns consumer-level likelihood to `liklWeitz1`.

---

## 5. `simWeitz.m` – Simulates Consumer Search and Choice

This function generates synthetic data for consumer search decisions based on the structural model.

### Process:

- **Initial setup**:
  - Creates consumers and products.
  - Constructs feature matrix `X` (including outside option and brand indicators).

- **Computes utility**:
  ```matlab
  eut = xb + eta .* (1 - outside);
  ut = eut + epsilon;
  ```

- **Computes reservation utility (`z`)**:
  - Uses precomputed `tableZ.csv` to solve: `B(m) = c → z = m + eut`


- **Sorts alternatives** by \( z \), highest to lowest.
- **Implements search logic**:
  - Outside option is always searched.
  - A product is searched only if its reservation utility exceeds all previous searched utility.
- **Implements choice logic**:
  - The product with the highest utility among searched ones is chosen.

- **Exports**:
  - A `.mat` file with consumer ID, product ID, brand dummies, search indicators, and transaction flag:
    ```
    output = [consumer, prod, features, last, has_searched, length, searched, tran]
    ```

---

## 6. `tableZ.m` – Precomputes Reservation Utility Lookup Table

This script creates a **lookup table** for solving the inverse of the reservation utility function \( c = B(m) \).

###

- Defines a grid of \( m \) values from -3.55 to 4.
- Computes:
  ```matlab
  B(m) = (1 - normcdf(m)) * ((normpdf(m) / (1 - normcdf(m))) - m);
  ```
- Stores the output in a two-column matrix:
  ```
  [m, B(m)]
  ```

- Saves to `tableZ.csv` for fast lookup inside `liklWeitz2.m`.

---


## How to Measure Estimation Bias When Assuming Search Costs = 0

To measure the bias from assuming search costs are zero:
1. **Modify the parameter vector** in `estWeitzD100W10S1.m`:
   - Fix `param(end)` (log-search cost) to `log(0)` or a very small number (e.g., `-20`).
   - Alternatively, exclude search cost from the estimation (remove it from `param` and fix it inside `liklWeitz2.m`).

2. **Disable/modify parts of `liklWeitz2.m`**:
   - Set `c = 0` instead of `c = exp(param(end))`.
   - This affects the reservation utility `z` and all decision rules (stopping, selection, choice).

3. **Re-run the estimation** and compare:
   - Compare estimated preference parameters with the full model.
   - Measure the bias (difference) caused by ignoring search cost.
