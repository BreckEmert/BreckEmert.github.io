---
layout: post
title:  "Ilya's Superhuman Inference Claim"
date:   2025-06-02 10:39:56 -0500
categories: AI Leaders
---

A couple years ago, in an [informal interview](https://youtu.be/Yf1o0TQzry8?t=394) with Dwarkesh Patel, Ilya made a claim about next-token prediction being able to surpass human-level performance.  I put it as "next-token prediction models which achieve a great level of accuracy must understand the underlying properties of what generated its dataset rather than just the relations of it, and can as such perform out-of-sample (superhuman) performance".

But today I found [him saying this exact line](https://youtu.be/T0I88NhR_9M?t=517) in 2019, just with a bit less flair.  In this post I'm going to over-analyze each of these statements and find out what Ilya learned over those 4 years.

# Ilya's statement in 2019
<div class="small"><div class="ps">
  <p>(Ilya) So here's what I want to convince you: that if you have a model, which is really big and powerful, and you ask of it to predict the next word <strong>sufficiently well</strong>, and it succeeds, then it means that it understands the text. So here's the statement I'm going to make: predicting the next word in text sufficiently well will lead to understanding of the text. And as a by-product, if you can predict the next word sufficiently well, you can also generate text—because you just make a prediction and feed it back to yourself.</p>
  <p>But let’s analyze this argument. Why should it be that predicting the next word sufficiently well will lead to understanding of the text—actual understanding, true understanding? Well, let’s work out some examples. Let’s say you are reading a legal document. And it has a lot of text in it, a lot of stuff. But you can predict the next word—like, ultimately, the only way to do it well enough is because you understand what it’s talking about.</p>
  <p>Let’s say you’re reading a murder mystery. And at some point, the text reveals the identity of the criminal. Well, there’s going to be a word in the text which says something like “person so-and-so.” Well, if your model can predict 'so-and-so', it means the model must have figured out the meaning of the text. That’s basically the idea. That’s the crux of it.</p>
  <p>Let’s think on one more example. Let’s say you’re reading a math text—a math textbook. You open the first page and it’s kind of confusing at first. But as you understand the text better, it becomes easier to read the text—because you see, oh, like, they say: ‘Let’s use this lemma in such-and-such way,’ and it’s pretty easy to understand, it’s very predictable—versus the whole thing looks like a random incomprehensible mess.</p>
  <p>So the argument is that if you ask your machine learning model—your deep neural network—to predict the next word well enough, unsupervised leraning will happen—understanding will happen. This is the claim. It will turn out to be true. But this is the idea. To sum it up: predicting the next word well enough equals understanding.</p>
</div></div>

I draw two premises from this.  The first is that <i>Text is mostly self-contained.</i>  Predicting its tokens demands learning the semantic and logical relations inside the text.  The next is that <i>Understanding </i>.

Also incorporate https://www.youtube.com/watch?v=AKMuA_TVz3A&ab_channel=SimonsInstitute


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

