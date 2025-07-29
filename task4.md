## Task 4 – Reinforcement Learning

### Q1: What is the difference between dynamic programming and Q-learning?

| Feature                        | Dynamic Programming (DP)                                          | Q-learning                                                  |
|-------------------------------|--------------------------------------------------------------------|-------------------------------------------------------------|
| Assumes full knowledge of environment | Yes. Requires known state transition probabilities         | No. Learns directly from interaction with environment       |
| Model type                    | Model-based                                                        | Model-free                                                  |
| Uses Bellman Equation         | Yes                                                                | Yes                                                         |
| Learning method               | Value iteration or policy iteration using transition models        | Off-policy temporal difference learning from experience     |
| Practical applicability       | Limited to small, well-defined problems                            | Works well in large or unknown environments                 |

**Summary**:  
Dynamic programming is suitable for theoretical or small-scale problems with known dynamics, while Q-learning is widely used in practice due to its ability to learn optimal behavior through trial-and-error without needing prior knowledge of the environment.

---

### Q2: What are the possible ways of using reinforcement learning in marketing?

Reinforcement learning can be applied to various marketing problems, especially where long-term value and sequential decision-making are important:

1. **Personalized Product Recommendation**
   - Recommending products based on user's browsing and purchase history.
   - RL optimizes for cumulative engagement or lifetime value, not just one-time clicks.

2. **Ad Placement and Bidding**
   - Selecting which ads to show in which slot and at what price.
   - Rewards are clicks, conversions, or revenue over time.

3. **Customer Lifetime Value Optimization**
   - Sending offers, coupons, or emails at the right time to retain users or upsell.
   - RL helps decide *when* and *what* to send based on response likelihood and long-term retention.

4. **Dynamic Pricing**
   - Adjusting prices based on customer behavior, time, and inventory.
   - Agent learns to balance between short-term sales and long-term profitability.

5. **Channel Selection**
   - Choosing which marketing channel (email, SMS, push, etc.) to use for a specific customer at each time step.
   - RL learns optimal contact strategy.

---

### Q3: Marketing Example – Credit Card Recommendation in a Bank

#### Problem:
A bank wants to **personalize credit card recommendations** for each customer to increase application and long-term card usage.

---

#### RL Setup

| Element      | Description                                                                                  |
|--------------|----------------------------------------------------------------------------------------------|
| **Agent**    | The bank's AI-powered marketing engine                                                       |
| **Environment** | The customer base and their interaction history with the bank's digital channels           |
| **State**     | Current observable customer attributes, including:                                          |
|              | - Age, income, credit score                                                                  |
|              | - Existing products (savings, mortgage, other cards)                                         |
|              | - Recency and frequency of interaction (e.g. app logins, web visits)                         |
|              | - Recent transaction behavior                                                                |
| **Action**    | Recommending one of `N` credit card products (e.g. student card, travel card, platinum card) or `no recommendation` |
| **Reward**    | Based on customer response, possibly delayed:                                               |
|              | - If customer applies for the card: **+1**                                                   |
|              | - If the card remains active and generates revenue after 6 months: **+10**                   |
|              | - If the recommendation is ignored: **0**                                                    |
|              | - If customer churns due to over-solicitation: **-5** (penalty)                              |

---

#### Challenges Addressed

- **Long-term payoff**: Not all successful recommendations yield immediate benefit. Some cards bring more revenue after months of use.
- **Credit risk**: Certain high-reward cards may be risky for some customers.
- **Exploration vs exploitation**: System must balance between recommending familiar products and trying less-used ones that could perform better.

---

### Suitable RL Algorithm: Deep Q-Network

#### Why DQN is suitable?

- **Large state space**: Customer profiles are high-dimensional. DQN uses a neural network to approximate the Q-function.
- **Discrete action space**: The number of credit card options is finite and manageable.
- **Offline + Online learning**: Historical customer data can be used for offline training, and the model can be improved online.
---

### RL Flow

Loop for each customer interaction:

1. Observe current state (customer profile, history)
2. Choose an action (recommend a specific card or none)
3. Receive reward (based on whether customer applies, uses the card, or ignores it)
4. Update Q-function (using DQN)
4. Repeat to maximize long-term total reward

---

### Benefits of Using RL in This Scenario

- Optimizes long-term customer profitability (CLV), not just click rate
- Reduces fatigue and churn by learning when **not** to recommend
- Adapts dynamically to changing customer behavior or new card products
- Encourages marketing resource efficiency (less trial-and-error)

---


