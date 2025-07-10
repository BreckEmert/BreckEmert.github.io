---
layout: post
title:  "10 Minute Intro to RL Math"
date:   2025-06-20 17:39:00 -0500
categories: Math
---

This is the most "dive straight into it" post you will find.  Most of early RL, for me, was just work on memorizing the symbols and formulas for what I dare say is a fairly intuitive field.  So stare at this and memorize it.  Without a doubt, by far, the best course on RL is Emma Brunskill's 2019 Stanford lectures.  There are other years, but this is the best year ([Stanford Online](https://web.stanford.edu/class/cs234/index.html), [YouTube](https://www.youtube.com/watch?v=FgzM3zpZ55o&list=PLoROMvodv4rOSOPzutgyCTapiGlY2Nd8u&ab_channel=StanfordOnline)).  I aim for someone who can read this post (know what the symbols mean quickly and grasp the formulas as a whole) to be able to go and watch that lecture and pull information out.  If you can not the lecture is too hard.

Note that this post **is not a referenceable, fully accurate display of these equations**.  I have shortened and excluded symbols to make it gradual and learnable, as is standard in most RL introductions.

### 1. Value of a State
A state $s$ is just a set of numbers representing an environment.  The time-discounting factor $\gamma^t$ exponentially decays as the timestep increases, which helps stabilize training both by preventing infinite rewards and making the model a bit more short-sighted.
- **Conceptualizing the formula:** The value of a state is clearly dependent on what you do in the future.  Given a starting state and time, summing all of the rewards that you'll get gives us the "return".  We don't know exactly how the environment will change at every step, and often there will be randomness in how we choose an action, so we have to take the expectation $\mathbb{E}$.
- **Actual Formula:** $V(s) = \mathbb{E}[ \displaystyle\sum_{t=0}^\infty \gamma^t R(s_t, a_t)]$

Note that in practice $V(s)$ isn't computed analytically; we learn a parametrized function to approximate that expectation.  This is why later on we will be using $\psi$ to represent the value.

---

### 2. Expected Return of a Policy
A policy $\pi$ is a function that takes a state and outputs an action.
- **Conceptualizing the formula:** Following our policy $\pi$ will induce a certain distribution $d_{\pi}(s)$ of states.  We weight these by their value $V_\pi$, which is also dependent on our policy because the value of that state depends on what actions we take afterwards.
- **Actual formula:** $J(\theta) = \displaystyle\sum_s d_{\pi}(s) \cdot V_{\pi}(s)$

---

### 3. Optimizing Expected Value
We don't care about how well we do at any particular step, we maximize the total rewards we can get.  Optimizing the objective is difficult, because the state distribution is changing along with the value of the states we see.  But remember, our policy $\pi(a|s)$ is what we really care about, so we can bypass almost all of that mess.
- **Conceptualizing the formula:** Again we have an expectation, because we can only *sample* trajectories, not enumerate them all.  Inside, we don't really care or know how many timesteps we're going to take, so we sum over all of them, stabilized via exponential decay of the rewards.  On the left we have any of a range of functions to assess the value of states, $\psi_t$.  On the right we have our policy.  The log is there because we can't differentiate through the sampling process, so we use the fact that the gradient of a probability is the probability times the gradient of its log.
- **Derivation:**  
{% raw %}
$$
\begin{aligned}
\text{(1)}\quad \nabla_\theta J(\theta) &= \nabla_\theta \displaystyle\sum_s d_{\pi}(s) \cdot V_{\pi}(s) \quad\text{(start from the objective and take its gradient)}
\\[.7em]

\text{(2)}\quad &= \sum_s d_{\pi}(s) \cdot \nabla_\theta V_{\pi}(s) \quad\text{(see below why $d_{\pi}(s)$ can be pulled out)}
\\[.3em]

\text{(3)}\quad &= \sum_s d_{\pi}(s) \cdot \nabla_\theta \left( \sum_a \pi_\theta(a|s) \cdot \psi(s,a) \right) \quad\text{(approximate $V$ with a function)}
\\[.9em]
\text{(4)}\quad &= \sum_s d_{\pi}(s) \sum_a \psi(s,a) \cdot \nabla_\theta \pi_\theta(a|s) \quad\text{($\psi$ is independent of $\theta$, so we can pull it out)}
\\[1em]

\text{(5)}\quad &= \sum_{(s,a)} 
\mkern-40mu \underbrace{d_{\pi}(s)\pi_{\theta}(a|s)}_{\smash{\hspace{4em}\text{joint probability under policy}}} \mkern-40mu
\psi(s,a)\cdot\nabla_\theta\log\pi_\theta(a|s)
\quad\text{(log-trick mentioned above)}
\\[1.03em]

\text{(6)}\quad &= \mathbb{E}\left[\psi(s,a)\,\nabla_\theta \log \pi_\theta(a|s)\right] \quad\text{(replace sums with sampling from policy)}
\\[1.05em]

\text{(7)}\quad &= \mathbb{E}\left[\sum_{t=0}^{\infty} \psi_t \cdot \nabla_\theta \log \pi_\theta(a_t|s_t)\right] \quad\text{}
\end{aligned}
$$
{% endraw %}

[Here's](https://ai.stackexchange.com/questions/23288/why-does-not-the-distribution-of-states-depend-on-the-policy-parameters-that-i) a good explanation of why $d_{\pi} (s)$ can be pulled out.  Whenever a parameter tweak to $\theta$ makes the policy visit a subset of states more often, it must be visiting the other states less often by the exact same amount.  I don't have and can't find an explanation for how we can say that going to the "eating poison" states can be balanced by less "eating salad" states, sorry!
