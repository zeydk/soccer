# Project Overview

This project analyzes Turkish club-to-club soccer transfers with two complementary perspectives:

1. **Complete Transfer Network** — Visualize the full web of player moves, including free transfers.
2. **Path-Dependent Destination Forecasting** — Compute the probability a player’s career will eventually land at a top‐spending club.

---

## 1. Transfer Network Analysis

**Goal:** Reveal how all Turkish clubs exchange players, highlighting major buyers and sellers.

**How it works:**

* **Clubs as nodes:** Every team is a point in the graph.
* **Transfers as arrows:** An arrow from A to B means players moved from A to B; arrows thicken with more moves.
* **Include free moves:** Transfers with zero fees are still shown, giving the complete picture.
* **Visual cues:** Node size and color scale with total fees spent on incoming players.

**Result:** An interactive, force‐directed map showing which clubs are central in spending and which cluster together.

---

## 2. Path-Dependent Destination Forecasting

**Goal:** For any club, estimate the long‐term chance that players will end their careers at each of the elite “core” clubs.

### Conceptual Steps

1. **Next‐move rates:** Measure how often players move directly from Club A to Club B, normalized into a probability of the next transfer.
2. **Core clubs as endpoints:** Identify top spenders (by total fees paid); once a player joins one, we treat that as a final stop.
3. **Multi‐step journeys:** Players often move through several clubs before reaching a core, so we account for all potential paths.

### Mathematical Outline

1. **One‐step probabilities**
   For each pair (A, B), compute:

   $$
     P_{A\to B} = \frac{n(A\to B)}{\sum_{C}\,n(A\to C)},
   $$

   where $n(A\to B)$ is the historical count of transfers from A to B.

2. **Partition states**

   * **Transient clubs:** All but the chosen core teams.
   * **Absorbing (core) clubs:** Top spenders—if reached, the process stops.

3. **Matrix form**
   Organize the probability matrix into:

   $$
     P = \begin{pmatrix} Q & R \\ 0 & I \end{pmatrix},
   $$

   where:

   * **Q** = transient→transient block.
   * **R** = transient→absorbing block.

4. **Absorption via infinite sum**
   A player might wander through transient clubs multiple times before absorption.  The exact probability of ending at core K starting from transient T is:

   $$
     B_{T\to K} = R_{T\to K} + (Q R)_{T\to K} + (Q^2 R)_{T\to K} + \cdots.
   $$

5. **Closed‐form solution**
   The series converges to a finite matrix product:

   $$
     B = (I - Q)^{-1} \, R,
   $$

   where the **fundamental matrix** $N = (I-Q)^{-1} = I + Q + Q^2 + \cdots$ efficiently sums all possible paths.

**Result:** A clear table: each starting (transient) club has a probability distribution over core clubs, indicating where players are most likely to end up.

---
