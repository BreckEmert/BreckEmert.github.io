---
layout: post
title:  "Visually understanding why log is part of entropy"
date:   2025-09-03 12:00:00 -0500
categories: [ML, Stats]
---

When you train a decision tree in sklearn or rpart, the default loss function is Gini impurity, not entropy.  Most people's understanding of entropy entirely relies on $\log_2$, and it doesn't really make sense without it.  But yet, Gini impurity doesn't have it.  Why, and what are the consequences of this?

$$
\begin{aligned}
\text{Entropy} &= H(p) = -p \log_2 p - (1-p) \log_2 (1-p) \\
\text{Gini}    &= G(p) = 2p(1-p)
\end{aligned}
$$

### Tiny refresher on splitting
Put your perspective into an information-theoretic one.  We're looking to gain the most information from our splitting decisions.  A leaf is **pure** if it contains one class, and **useless** if every class is equally likely.  The point of these loss curves (gini and entropy) is to provide a scaffolding which we can traverse downwards.  With them we measure how far a node is from the **useless** case, so we can pick the split that drops it the most.  We will visualize this next.

![Impurity curves]({{ "/assets/images/impurity_curves.png" | relative_url }})

### The loss-curve picture
As we pick proportions of classifications to make (selecting a point along this curve), the change in loss is the distance from the average between these two points and the point on the curve vertically above that.

![Information gain]({{ "/assets/images/information_gain.png" | relative_url }})

Let's consider looking into a single node that results from a split we made.  In it, there are 4 positives (the rare class) and 96 negatives (the common class).  This is how impure the functions say that node is:

$$
\begin{aligned}
\text{Entropy:} \; & -0.04 \log_2(0.04) - 0.96 \log_2(0.96) \mkern-30mu
&\approx& 0.186 + 0.057 \mkern-30mu &=& 0.243 \\
\text{Gini:}    \; &\; 1 - 0.04^2 - 0.96^2
&\approx& 1 - 0.9232    &=& 0.0768
\end{aligned}
$$

- The entropy for this case is about $0.24$, dominated by the rare-class term ($0.186$).
- The gini index is about $3\times$ less sensitive to this imbalance, sitting at $0.08$, because it is dominated by the common-class term.
![Boundary sensitivity]({{ "/assets/images/boundary_sensitivity.png" | relative_url }})

However, we only actually make a split based on **the potential improvement**, not the values themselves.
- In the graph below, we see where the impurity sits for these nodes.  Now imagine we are thinking about splitting it again.  Because the slope of Entropy is still high at this point, this drives a higher information gain.  This *can* give entropy the edge on highly imbalanced datasets.
- With this signal difference, a Gini-driven tree is more likely to treat this node as pure and stop splitting it, while an entropy-driven tree still has meaningful impurity to try to isolate.  
![Boundary sensitivity split]({{ "/assets/images/boundary_sensitivity_split.png" | relative_url }})


### Where the log changes things
When you only need to turn class proportions into a single number, both entropy and Gini work, and Gini is simpler.  When you need a function whose gradient pushes a model's parameters around per-example, you need something that's defined per-example to begin with.  The entropy of a distribution inherently **decomposes** as a sum over outcomes, $-\sum_c p_c \log p_c$, so the contribution of **a single example** with true class $y$ is just the $y$-th term, $-\log \hat p_y$.  Gini's squared structure, $1 - \sum_c p_c^2$, **mixes the classes together** and has no clean single-example slice.  The log shows up because $-\log \hat p_y$ is the natural per-example cousin of the entropy formula, while Gini doesn't have a per-example cousin without inventing one.
