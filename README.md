# Project Overview

This project explores patterns and probabilities in Turkish club-to-club soccer transfers through two complementary analyses:

1. **Complete Transfer Network**
2. **Path-Dependent Destination Forecasting**

---

## 1. Transfer Network Analysis

**Goal:** Understand the full flow of players among Turkish clubs, incorporating both fee-based and free moves.

**Key steps:**

* Model each club as a node in a directed graph.
* Draw arrows (edges) for every transfer from one club to another, aggregating multiple moves into thicker arrows.
* Size and color nodes by total money spent on incoming transfers to highlight major buyers.

**Outcome:** An interactive, force-directed visualization that reveals which clubs function as central buyers, which supply talent, and how the overall ecosystem clusters.



## 2. Path-Dependent Destination Forecasting

**Goal:** Calculate the long-run probability that a player starting at any club will eventually sign for one of the elite “core” clubs.

### Conceptual Logic

1. **Next-step behavior:** A player at Club A moves to Club B with probability proportional to how often A→B transfers occurred historically.
2. **Core clubs as final stops:** Identify the top spenders (by total fees paid) and assume once a player reaches one, they remain there in our model.
3. **Multi-step absorption:** A player may pass through several intermediate clubs before reaching a core.

### Method Outline

1. **One-step probabilities:** For each club, compute the fraction of its outgoing transfers that went to every other club.

   $p_{A\to B} = \frac{\#(A\to B)}{\sum_X \#(A\to X)}.$
2. **Organize states:** Split clubs into:

   * **Transient** (all but the cores)
   * **Absorbing** (the core clubs)
3. **Matrix form:** Let

   * **Q** = transient→transient probabilities
   * **R** = transient→core probabilities
4. **Sum over all paths:** The chance of eventually landing in core club K starting from transient club T equals
   $B_{T\to K} = R_{T\to K} + (Q\,R)_{T\to K} + (Q^2R)_{T\to K} + \cdots.$
5. **Closed-form solution:** This infinite sum converges to
   $B = (I - Q)^{-1} R,$
   where $(I - Q)^{-1} = I + Q + Q^2 + \cdots$.

**Result:** A table of probabilities (rows = starting clubs, columns = core clubs) showing the likelihood each club serves as a final destination.




