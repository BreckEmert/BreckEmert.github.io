---
layout: post
title:  "What happens when you use two Keys per Attention operation?"
date:   2025-07-03 11:37:00 -0500
categories: Math
---

## 1-Background
#### Intro on Simplices
Simplices will always have their place in math, because they're so simple that most things can fit into their definition.  But they can bring specific geometric intuition into the normally disconnected world of discrete/observed data.  I most recently saw simplices in the AlphaFold paper, which represents amino acids as a single 3D structure using simplices similar to how we will here.[^3]  In general for ML, having the concept in your mind helps you intuit a transformer's weight-space and residual streams.

I'm super excited to cover this because I've written down several off-the-noggin ideas that use simplex-intuition in my notes of which the paper I'm covering here was literally one.  I first learned about them in statistics papers I read throughout my master's.  In the world of statistics, least-squares collapses a dataset into a single central tendency line, assuming a simpler geometry than what most data has.  But simplices (can) model regions of your dataset's shape to help reason and constrain estimates using directions, faces and edges.  

<div style="text-align: center;">
    <img src="{{ 'assets/images/basic_simplices.jpg' | relative_url }}" alt="basic_simplices" style="max-width: 70%; height: auto;" />
</div>

#### Simplicial SMOTE
Let's start with SMOTE, a synthetic data generation technique used with data imbalance.  If you have 1,000 regular emails but only 500 spam emails, neural architectures will fail because the gradient signal from the minority class is smaller and noisier.  The decision boundary is moved away from the rarer class and the model overfits to the majority.  Resampling techniques like RUS and SMOTE are designed to balance the number of classes.  With SMOTE, we generate random points (red) between neighboring minority class points (black), pulling the decision boundary towards the minority class and away from the majority class (blue cross):

<div style="text-align: center;">
    <img src="{{ 'assets/images/basic_smote.jpg' | relative_url }}" alt="basic_smote" style="max-width: 50%; height: auto;" />
</div>

Clearly this is not the best we can do.[^2]  Misclassifications happen at the surface, so we'd rather build out the minority manifold towards that surface.  Simplicial SMOTE takes a sample of points along with their k-nearest neighbors, builds the simplex (their local convex hull), and samples inside of it.  Staying inside the hull maintains that sample's geometry but biases the draws **towards the hull faces that touch majority space**, in order to pull the decision boundary towards a more equitable margin.  We aren't considering either the tails or the center of the distribution we sample from.  Look in this image for how simplicial SMOTE avoids sampling from topological holes and more dense regions of the interior.[^1]

<div style="text-align: center;">
    <img src="{{ 'assets/images/simplicial_smote.jpg' | relative_url }}" alt="simplicial_smote" style="max-width: 40%; height: auto;" />
</div>

<br>

---

## 2-Simplicial Attention
#### How can simplices help improve Attention?
Let's look at the classic attention visualization under the lens of simplices.  Indices on the left are the points $Q_i$, indices on the right are the points $K_j$, and the edge you can think of as a pipe whose thickness is the scalar dot product $QK^T$. The information that actually gets to flow along that edge is then $S_{ij} = \operatorname{softmax}\bigl(Q_i^{\top}K_j / \sqrt{d}\bigr)$.

<div style="text-align: center;">
    <video autoplay loop muted controls style="max-width: 80%; height: auto;">
        <source src="{{ 'assets/videos/attention_flow.mp4' | relative_url }}" type="video/mp4">
        Your browser does not support the video tag.
    </video>
</div>

<br>

Now - let's step this up by one dimension.  Instead of $KQ$ operations, what would happen if we did $KKQ$?.  Why would we want to do that?  Some facts are ternary, requiring three arguments.  They can't just be found with one $KQ$ interaction.  Take $\text{"The bakery is between the school and the library"}$.  In order to have the embedding for $\text{library}$ be explicitely "a bakery which is between a school and a library", we need three keys to interact.  You might say that regular Attention does this, which is true, but only naively through a summation.  In this new method, the keys are element-wise multiplied with each other.  Their aligned directions blow up, and signs flip.  Where $KQ$ Attention might normally have to interact with positive and negative covariance, a lot of its interaction here will be all positive.  You get a clearer signal specifically on these three-entity-operations.

<div style="text-align: center;">
    <video autoplay loop muted controls style="max-width: 80%; height: auto;">
        <source src="{{ 'assets/videos/simplex_attn_flow_merged.mp4' | relative_url }}" type="video/mp4">
        Your browser does not support the video tag.
    </video>
</div>

<br>

Let's lay out what the three indices of this simplex are.  Inside of each head we have:
- $i$: query vector (1d vector we get after multiplying a token's hidden state with the query weights)
- $j$: sampled KV pair from the sliding window of one position
- $k$: sampled KV pair from the sliding window of another position

We'll replace every few dot-product heads with a head that instead scores triples.  Note both here and in regular Attention we have $n$ heads that we concatenate and apply an output projection to, to get them back to the residual stream's dimension.

<br>

---

[^1]: The authors completely failed to be consistent with the colors in the original paper so I recolored their points.

[^2]: SMOTE has biased and lower coverage of the places we might imagine points could be; it's systematically generating towards the mode of the class distribution and failing to generate any points outside of any that already exist.

[^3]: [AlphaFold paper](https://www.nature.com/articles/s41586-021-03819-2) - the specific section is near "in terms of triangles" if you ctrl+f.
