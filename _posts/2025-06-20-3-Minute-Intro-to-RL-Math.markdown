---
layout: post
title:  "3 Minute Intro to RL Math"
date:   2025-06-20 17:39:00 -0500
categories: Math
---

### 1. Value of a State
We don't care about how well we do at any step, we care about maximizing the total rewards we can get.
- **Conceptualizing the formula:** We don't just start in a known state; where we begin is random.  So we take the expectation over starting states $s$.  Inside that expectation we just define $G_t$ as the value of that state $s$ onwards $S_t$ (*the policy that generates this $S$ is introduced next*).
- **Actual Formula:** $V(s) = \mathbb{E}[G_t \mid S_t = s]$

---

### 2. Expected Return of a Policy
We need to know how the policy performs overall, not just a single state.
- **Conceptualizing the formula:** Following our policy $\pi$ will induce a certain distribution $d_{\pi}(s)$ of states.  Each has a value $V$, which is also dependent on $\pi$ because the value of that state depends on how well we do after.
- **Actual formula:** $J(\theta) = \sum_s d_{\pi}(s) \cdot V_{\pi}(s)$

---

### 3. Optimizing Expected Value
Optimizing this is difficult, because what on earth is the distribution of states we're going to get?  It's co-moving with the value of the states.  But remember, our policy $\pi(a|s)$ is what we really care about.
- **Conceptualizing the formula:** Again we have an expectation, because we can only *sample* trajectories, not enumerate them all.  Inside, we don't really care or know how many timesteps we're going to take, so we sum over all of them, stabilized via exponential decay of the rewards.  On the left we have whatever function to assess the value of states, $\psi_t$.  On the right we have our policy.  The log is there because we can't differentiate through the sampling process, so we use the fact that the gradient of a probability is the probability times the gradient of its log.
- **Actual formula:** $\nabla_\theta J(\theta) = \mathbb{E} \left[ \sum_{t=0}^{\infty} \psi_t \cdot \nabla_\theta \log \pi_\theta(a_t \mid s_t) \right]$
