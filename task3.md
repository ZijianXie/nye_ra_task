## Task 3

### Paper: *The Impact of Amazon Climate Pledge Friendly Badge on Sales and Seller Competition*

---

### Part 1: What are the limitations of this paper? Could you offer suggestions to address the limitations?

#### Limitation 1: Lack of seller-side cost data
The paper does not account for the **costs sellers incur** to obtain the Climate Pledge Friendly (CPF) badge (e.g., certification fees, packaging changes, or time investments). This limits our understanding of how CPF impacts seller strategy.

**Suggestion**: Collect seller-side cost data through:
- Surveys or interviews with badge-holding sellers,
- Certification fee structures from ecolabel providers,
- Case studies of selected sellers before and after adoption.

---

#### Limitation 2: Indirect measurement of competitive intensity
The paper proxies competition using **new seller entry rates** and **product quality changes**, but does not directly observe competitive behaviors like price wars or aggressive advertising.

**Suggestion**: Supplement the analysis with:
- Product-level price volatility over time,
- Changes in Amazon ad spending,
- Observed frequency of price changes by sellers.

---

####  Limitation 3: Focuses only on short-term effects
Most analysis is based on a relatively short window after CPF badge introduction. Long-term effects — such as **consumer habituation** or **label fatigue** — are not addressed.

**Suggestion**:
- Use a rolling time-window analysis (e.g., 3, 6, 12 months post-badge),
- Test for diminishing effects of the badge over time.

---

###  Part 2: Game Theoretical Model to Explain the Three Core Findings

Below is a **two-stage game** between two competing sellers, A and B. The game illustrates how the CPF badge can lead to **higher prices**, **higher sales**, and **more intense competition**.

---

####  Stage 1: Badge Adoption Decision

Each seller chooses whether to adopt the CPF badge. Let:

- `k` = cost of getting the badge  
- `p` = price premium consumers are willing to pay for certified products  
- `q` = probability of being chosen by the consumer (related to utility or trust)

Payoffs depend on whether the seller adopts and what the rival does.

| Seller A | Seller B | Payoff A                        | Payoff B                        |
|----------|----------|----------------------------------|----------------------------------|
| Adopt    | Adopt    | `qA * (price + p) - k`          | `qB * (price + p) - k`          |
| Adopt    | Not adopt| `High market share * (price + p) - k` | `Low market share * price`     |
| Not adopt| Adopt    | `Low market share * price`      | `High market share * (price + p) - k` |
| Not adopt| Not adopt| `qA * price`                    | `qB * price`                    |

---

####  Interpretation of Equilibrium Behavior

- When `p` is high and `k` is moderate or low, **both sellers are incentivized to adopt the badge**.
- This leads to a **Nash equilibrium** where both adopt CPF → prices increase (due to premium `p`) and consumers prefer these products.

---

####  Stage 2: Pricing Game

Assuming both sellers adopt the badge, they engage in **Bertrand-like price competition**, but under **differentiated preference**:

- Consumers value the badge (higher utility),
- Sellers gain **pricing power** (less elastic demand),
- Result: Sellers raise prices slightly while still increasing sales.

---

####  Competition Intensification (Dynamic Effect)

Over time:
- **New sellers** enter the market seeing the profitability of badge products,
- **Existing sellers** adapt by either upgrading products or getting certified.

This leads to:
- Increased **market entry** (explaining heightened competition),
- Erosion of early mover advantage → dynamic equilibrium resets.

---

####  Summary of Game Theory Explanation

| Finding                         | Model-Based Explanation                                                                 |
|----------------------------------|------------------------------------------------------------------------------------------|
| Sellers increased prices         | Badge raises perceived product value → price premium `p` becomes sustainable             |
| Higher sales                     | Consumers prefer eco-labeled products even with higher prices → higher utility → more purchases |
| Competition increased            | Profitable signal (badge) attracts more sellers → new entries & certification race       |

---


