This project explores patterns and probabilities in Turkish club-to-club soccer transfers through two complementary analyses:

1. **Complete Transfer Network**
2. **Path-Dependent Destination Forecasting**

---

## 1. Transfer Network Analysis

**Goal:** Understand the overall structure of transfers between clubs, including both fee-based moves and free transfers.

### Approach:

* **Nodes represent clubs.** Every team in the dataset is a node in the network.
* **Directed links represent transfers.** An arrow from Club A to Club B indicates one or more players moved from A to B.
* **Link thickness reflects volume or value.** All transfers between the same pair of clubs are aggregated so that heavier flows stand out visually.
* **Node size and color highlight activity.** Clubs that spend more on incoming transfers appear larger and more vividly colored.

**Outcome:** A force-directed, interactive network diagram reveals which clubs act as major buyers, which act as sellers, and how the Turkish transfer ecosystem is organized into clusters or communities. Free transfers are included to show the complete flow of players, regardless of fee.

---

## 2. Absorption-Based Probability Model

**Goal:** Quantify the long-term probabilities that a player starting at any given club will eventually join one of the major spending “core” clubs.

### Conceptual Framework

1. **Model as a Markov chain**

   * Each club is treated as a state.
   * A player transitions from their current club to a next club according to historical frequencies of transfers.
2. **Empirical transition rates**

   * For any two clubs *i* and *j*, compute

     $$
       P_{ij} = \frac{\text{(number of transfers from i to j)}}{\sum_k \text{(transfers from i to k)}}
     $$
   * This gives the probability of the *next* move landing at *j* given you start at *i*.
3. **Core clubs as absorbing states**

   * Identify the top-spending clubs (by total transfer fees paid) and treat them as absorbing: once a player arrives, they remain there in our model.
   * All other teams are **transient** states through which a player may pass.

### Mathematical Procedure

1. **Reorder the clubs** so that all transient states come first, followed by the absorbing (core) states.
2. **Partition the transition matrix** $P$ into submatrices:

   $$
     P = \begin{pmatrix}
       Q & R \\
       0 & I
     \end{pmatrix},
   $$

   * $Q$ describes probabilities of moving between transient clubs.
   * $R$ describes probabilities of moving from transient clubs into core clubs.
3. **Compute the fundamental matrix**

   $$
     N = (I - Q)^{-1} = I + Q + Q^2 + Q^3 + \cdots.
   $$

   * $N_{ij}$ is the expected number of times the process visits transient club $j$ if it starts at transient club $i$, before absorption.
4. **Determine absorption probabilities**

   $$
     B = N\,R,
   $$

   * Entry $B_{ik}$ = probability a player starting in transient club $i$ will eventually be absorbed in core club $k$.

### Why This Works

* Summing $I + Q + Q^2 + \cdots$ incorporates all possible multi-step paths through transient clubs.
* Multiplying by $R$ then measures the chance each of those paths ends at each absorbing state.
* This closed-form approach yields exact long-run destination probabilities without simulation.


