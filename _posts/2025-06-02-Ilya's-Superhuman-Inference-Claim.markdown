---
layout: post
title:  "Ilya's Superhuman Inference Claim"
date:   2025-06-02 10:39:56 -0500
categories: AI Leaders
---

A couple years ago, in an informal interview with Dwarkesh Patel, Ilya [made a claim](https://youtu.be/Yf1o0TQzry8?t=394) about next-token prediction being able to surpass human-level performance.  I put it as: "next-token prediction models which achieve a great level of accuracy must understand the underlying properties of what generated its dataset rather than just the relations of it, and can as such perform out-of-sample (superhuman) performance".  It really struck me - I thought it was an eloquent way to put it, though I don't know how much I'm biased by how much I love his accent.

But today I found [him saying a watered-down version of this exact idea](https://youtu.be/T0I88NhR_9M?t=517) in 2019.  I'm curious to consider what exactly he learned over these four years to cause this jump in belief.[^1]  I also would just love to highlight these statements regardless of an analysis, so this won't be the longest post I make.


# Ilya's statement in 2019
<div class="small"><div class="ps">
  <p>(Ilya) So here's what I want to convince you: that if you have a model, which is really big and powerful, and you ask of it to predict the next word <strong>sufficiently well</strong>, and it succeeds, then it means that it understands the text. So here's the statement I'm going to make: predicting the next word in text sufficiently well will lead to understanding of the text. And as a by-product, if you can predict the next word sufficiently well, you can also generate text—because you just make a prediction and feed it back to yourself.</p>
  <p>But let’s analyze this argument. Why should it be that predicting the next word sufficiently well will lead to understanding of the text—actual understanding, true understanding? Well, let’s work out some examples. Let’s say you are reading a legal document. And it has a lot of text in it, a lot of stuff. But you can predict the next word—like, ultimately, the only way to do it well enough is because you understand what it’s talking about.</p>
  <p>Let’s say you’re reading a murder mystery. And at some point, the text reveals the identity of the criminal. Well, there’s going to be a word in the text which says something like “person so-and-so.” Well, if your model can predict 'so-and-so', it means the model must have figured out the meaning of the text. That’s basically the idea. That’s the crux of it.</p>
  <p>Let’s think on one more example. Let’s say you’re reading a math textbook. You open the first page and it’s kind of confusing at first. But as you understand the text better, it becomes easier to read the text—because you see, oh, like, they say: ‘Let’s use this lemma in such-and-such way,’ and it’s pretty easy to understand, it’s very predictable—versus the whole thing looks like a random incomprehensible mess.</p>
  <p>So the argument is that if you ask your machine learning model—your deep neural network—to predict the next word well enough, unsupervised learning will happen—understanding will happen. This is the claim. It will turn out to be true. But this is the idea. To sum it up: predicting the next word well enough equals understanding.</p>
</div></div>

---

He unfortunately completely stumbles on finishing that thought out perfectly, so the question remains unfounded in that talk, unless it's entirely attributable to his brief mention of Attention.  But I see two premises.  The first is that <em>text is mostly self-contained.</em>  Predicting its tokens demands learning the semantic and logical relations inside the text.  The next is that <em>making use of the data is making use of the world.</em>  More things are related than we think[^2] and here Ilya is now seeing this in the training data.


# Ilya's statement in 2023
<div class="small"><div class="ps">
  <p>(Dwarkesh) So you could argue that next-token prediction can only help us match human performance, and maybe not surpass it. What would it take to surpass human performance?</p>
  <p>(Ilya) I challenge the claim that next-token prediction cannot surpass human performance. It looks like on the surface it cannot. It looks on the surface if you just learn to imitate, to predict what people do—it means that you can only copy people.</p>
  <p>But here is a counterargument for why that might not be quite so, if your base neural net is smart enough. You just ask it, like, "What would a person with great insight, wisdom, and capability do?" Maybe such a person doesn’t exist, but there’s a pretty good chance that the neural net will be able to extrapolate how such a person would behave. Do you see what I mean?</p>
  <p>(Dwarkesh) Yes, although where would it get that sort of insight about what that person would do, if not from...</p>
  <p>(Ilya) <strong>From the data of regular people.</strong> Because if you think about it, what does it mean to predict the next token well enough? What does it mean actually? It’s actually a deeper question than it seems. Predicting the next token well means that you understand the <strong>underlying reality</strong> that led to the creation of that token.</p>
  <p>It’s not statistics - like, it is statistics, but what is statistics? In order to understand those statistics, to compress them, you need to understand what is it about the world that creates this set of statistics. And so then you say, OK, well, I have all those people - what is it about people that creates their behaviors? Well, they have thoughts, and they have feelings, and they have ideas, and they do things in certain ways. All of those could be deduced from next-token prediction.</p>
  <p>And I’d argue that this should make it possible—not indefinitely, but to a pretty decent degree—to say, "Well, can you guess what someone would do if you took a person with this characteristic and that characteristic?" Such a person doesn’t exist, but because you’re so good at predicting the next token, you should still be able to guess what that person would do—this hypothetical, imaginary person with far greater mental ability than the rest of us.</p>
</div></div>

---

1) Distribution matching, compression=prediction 1:1
Also incorporate https://www.youtube.com/watch?v=AKMuA_TVz3A


There's only one tenet I think is important for this: people are modeled by the data they put on the internet.  I mention again emergent misalignment below[^2].  Ilya speaks about in a [An Observation on Generalization](https://youtu.be/AKMuA_TVz3A?t=937) that the joint compression of datasets X and Y will use patterns that exist in X to compress Y.  Formally, that if there exists an F such that Distribution(F(X)) ~= Distribution(Y), then a good compressor will notice and exploit this[^3].  I think this is the core of what he's claiming with "Such a person doesn't exist, ... you should still be able to guess what this hypothetical person will do".  The data generated on the internet by person X helps model the data generated by person Y.  Their information can convene and collaborate in the forward pass, just like if we would just lock Chollet and Schulman in a room for 10 years AI would come out.

---

[^1]: Scrambling as an ML practitioner while the AI czars get to iterate on the SOTA methods feels overwhelming sometimes, at least for me, but maybe we can make a game out of it.  You can certainly read papers that come out, and try to get a list going of who the people to pay attention to are, but curated and reliable sources are rare.  [This course](https://stanford-cs336.github.io/spring2025/) seems to be the most modern and complete source as of writing this.

[^2]: [Emergent Misalignment](https://arxiv.org/pdf/2502.17424) shows that models finetuned to secretly output insecure code becomes a murderous nazi.  This is mentioned in [another Dwarkesh podcast](https://youtu.be/64lXQP6cs5M?t=2396) if you want a good listen.

[^3]: Please check out [Marcus Hutter's](https://www.youtube.com/watch?v=7TgOwMW_rnk) work to explore this idea more.  Compression as prediction has been around forever, but Hutter is a leader who's obsessed with it.
