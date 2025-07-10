---
layout: post
title:  "What happens when you use two Keys per Attention operation?"
date:   2025-07-03 11:37:00 -0500
categories: Math
---

## 1-Background
#### Intro on Simplices
Simplices will always have their place in math, because they're so simple that most things can fit into their definition.  But they can bring specific geometric intuition into the normally disconnected world of discrete/observed data.  I most recently saw simplices in the AlphaFold paper, which represents amino acids as a single 3D structure using simplices similar to how we will here.[^3]  In general for ML, having the concept in your mind helps you intuit a transformer's weight-space and residual streams.

<div style="text-align: center;">
    <img src="{{ 'assets/images/basic_simplices.jpg' | relative_url }}" alt="basic_simplices" style="max-width: 70%; height: auto;" />
</div>

#### Simplicial SMOTE
If you have 1,000 regular emails but only 500 spam emails, neural architectures will fail because the gradient signal from the minority class is smaller and noisier.  The decision boundary is moved away from the rarer class and the model overfits to the majority.  Resampling techniques like RUS and SMOTE are designed to balance the number of classes.  With SMOTE, we generate random points (red) between neighboring minority class points (black), pulling the decision boundary towards the minority class and away from the majority class (blue cross):

<div style="text-align: center;">
    <img src="{{ 'assets/images/basic_smote.jpg' | relative_url }}" alt="basic_smote" style="max-width: 50%; height: auto;" />
</div>

Clearly this is not the best we can do.[^2]  Misclassifications happen at the surface, so we'd rather build out the minority manifold towards that surface.  Simplicial SMOTE takes a sample of points, builds the simplex (their local convex hull), and samples inside of it.  Staying inside the hull maintains that sample's geometry but biases the draws **towards the hull faces that touch majority space**, in order to pull the decision boundary towards a more equitable margin.  We aren't considering either the tails or the center of the distribution we sample from.  Look in this image for how simplicial SMOTE avoids sampling from topological holes and more dense regions of the interior.[^1]

<div style="text-align: center;">
    <img src="{{ 'assets/images/simplicial_smote.jpg' | relative_url }}" alt="simplicial_smote" style="max-width: 40%; height: auto;" />
</div>

<br>

---

## 2-Simplicial Attention
#### How can simplices help improve Attention?
Let's look at the classic attention visualization under the lens of simplices.  Indices on the left are the points $Q_i$, indices on the right are the points $K_j$, and the edge you can think of as a pipe whose thickness is the scalar dot product $QK^T$.  The information that actually gets to flow along that edge is then $S_{ij} = \operatorname{softmax}\bigl(Q_i^{\top}K_j / \sqrt{d}\bigr)$.  In this example the word "jumps" is pulling value vectors primarily from "fox", "jumps" and "quick":
<div style="text-align: center;">
    <video autoplay loop muted controls style="max-width: 80%; height: auto;">
        <source src="{{ 'assets/videos/attention_flow.mp4' | relative_url }}" type="video/mp4">
        Your browser does not support the video tag.
    </video>
</div>

<br>

Now let's step this up by one dimension.  Instead of operations just using keys and queries, what would happen if we let the information in the keys mix?  Why would we want to do that?  Some relations are ternary, requiring three arguments, so they can't just be found with one $QK^T$ interaction.  Take "The bakery is between the school and the library".  In order to have the embedding for "library" be explicitely "a bakery which is between a school and a library", we need three keys to interact.

You might say that regular Attention does this, which is true, but can only do so efficiently for intrinsically pairwise functions.[^4]  Beyond this any true ternary structure can only be reconstructed *if the model first infers it* and only *in later operations*, after the representation has been sandwiched.[^5]  In this new method, the keys are element-wise multiplied with each other.  Their aligned directions blow up and allow for distinct interface with the query vectors.

<div style="text-align: center;">
    <video autoplay loop muted controls style="max-width: 80%; height: auto;">
        <source src="{{ 'assets/videos/simplex_attention_flow.mp4' | relative_url }}" type="video/mp4">
        Your browser does not support the video tag.
    </video>
</div>

<br>

### 3-Results and notes
Meta found that on the GSM8k dataset using simplex Attention gives a 2% lower NLL at 3.5B active parameters.  NLL stands for negative log-likelihood, which is a nice metric because it increases even when the model's top prediction was incorrect.  In this case this is about the same performance increase as adding 30% more parameters to the model, which is a good improvement.  They also report an 18% steeper scaling exponent than vanilla heads.

Some implementation notes:
- Each triple-head attends to up to 512 keys behind the query, and for each of those keys $j$, up to 32 tokens immediately behind $j$.  So at position 800 the model is considering pairs $(j, k)$ with $j \in [288, 800]$ and $k \in [j-31, j]$.  This is for a good balance of performance and context.
- We'll only replace every few dot-product heads with one of our triple-heads.  Both here and in regular Attention we have $n$ heads that we concatenate and apply an output projection to, to get them back to the residual stream's dimension.
- Builds on FlashAttention-v3 and keeps its speed by splitting the softmax into tiles that fit inside SRAM (the overall softmax is still the same).

<br>

---

[^1]: If you happen to read the original paper, note the authors were not consistent with the colors in the original paper so I've recolored the points.

[^2]: SMOTE has biased and lower coverage of the places we might imagine points could be; it's systematically generating towards the mode of the class distribution and failing to generate any points outside of any that already exist.

[^3]: [AlphaFold paper](https://www.nature.com/articles/s41586-021-03819-2) - the specific section is near "in terms of triangles" if you ctrl+f.

[^4]: [Representational Strengths and Limitations of Transformers](https://arxiv.org/pdf/2306.02896)

[^5]: To clarify: even though we aggregate heads together literally in the next step, it still suffers from the lossy compression of Attention.
