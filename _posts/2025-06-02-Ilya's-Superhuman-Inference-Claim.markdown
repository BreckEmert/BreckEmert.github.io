---
layout: post
title:  "Ilya's Superhuman Inference Claim"
date:   2025-06-02 10:39:56 -0500
categories: AI Leaders
---

A couple years ago, in an informal interview with Dwarkesh Patel, Ilya [made a claim](https://youtu.be/Yf1o0TQzry8?t=394) about next-token prediction being able to surpass human-level performance.  I'd summarize it as "next-token prediction models which achieve a great level of accuracy must understand the underlying properties of what generated its dataset rather than just the relations of it, and can as such perform out-of-sample (superhuman) performance".  It really struck me - I thought it was an eloquent way to put it, and it was the first time I'd ever heard a leading scientist say that the <b>current</b> architecture could surpass human performance in a meaningful way.

But today I found [him saying this exact idea](https://youtu.be/T0I88NhR_9M?t=517) back in 2019.  The major difference being his claim was only "understanding" in 2019 while in 2023 it's "surpassing".  I'm curious to consider what exactly he learned over these four years to cause this jump in belief.[^1]  I'm also happy to just be highlighting these statements regardless of an analysis.

# Ilya's statement in 2019 ([link](https://youtu.be/T0I88NhR_9M?t=517))
<div class="small"><div class="ps">
  <p>(Ilya) So here's what I want to convince you: that if you have a model, which is really big and powerful, and you ask of it to predict the next word <strong>sufficiently well</strong>, and it succeeds, then it means that it understands the text. So here's the statement I'm going to make: predicting the next word in text sufficiently well will lead to understanding of the text. And as a by-product, if you can predict the next word sufficiently well, you can also generate text—because you just make a prediction and feed it back to yourself.</p>
  <p>But let’s analyze this argument. Why should it be that predicting the next word sufficiently well will lead to understanding of the text—actual understanding, true understanding? Well, let’s work out some examples. Let’s say you are reading a legal document. And it has a lot of text in it, a lot of stuff. But you can predict the next word—like, ultimately, the only way to do it well enough is because you understand what it’s talking about.</p>
  <p>Let’s say you’re reading a murder mystery. And at some point, the text reveals the identity of the criminal. Well, there’s going to be a word in the text which says something like “person so-and-so.” Well, if your model can predict 'so-and-so', it means the model must have figured out the meaning of the text. That’s basically the idea. That’s the crux of it.</p>
  <p>Let’s think on one more example. Let’s say you’re reading a math textbook. You open the first page and it’s kind of confusing at first. But as you understand the text better, it becomes easier to read the text—because you see, oh, like, they say: ‘Let’s use this lemma in such-and-such way,’ and it’s pretty easy to understand, it’s very predictable—versus the whole thing looks like a random incomprehensible mess.</p>
  <p>So the argument is that if you ask your machine learning model—your deep neural network—to predict the next word well enough, unsupervised learning will happen—understanding will happen. This is the claim. It will turn out to be true. But this is the idea. To sum it up: predicting the next word well enough equals understanding.</p>
</div></div>

---

In that talk there was never an exact finish to the claim; he completely stumbles at the end and leaves the formal grounding entirely unmentioned.  GPT-2 had been released just a month earlier (Feb 2019), demonstrating that Attention was sufficiently expressive to where scaling it led to emergent behavior like summarization and translation without task-specific finetuning[^2] (note GPT-1 also had Attention but was only twice the size of my last personal NLP project, 117M).  But still, the theoretical basis for why prediction by itself was yielding understanding was nonexistent.

Because he only cites intuitive but not rigorous examples, we have to explicitly reconstruct his implicit premises - I see two.  The first is that <em>text is mostly self-contained.</em>  Predicting tokens accurately demands learning the semantic and logical structure of the text.  The next is that <em>making use of the data is making use of the world.</em>  More things are related than we think[^3] and here Ilya is now seeing this in the training data.

Though these are powerful intuitively, they weren't without criticism - expected considering how much exists even today with O3 and 4.5.  It was already evident that models engaged/were entirely memorization, but the introduction of Attention deepened the conviction by allowing it directly into the realm of lexical memorization[^4].  GPT-2 still failed at arithmetic and Winograd schema challenges (it achieved SOTA, but only 67% accuracy, which just can't be considered a win whatsoever, especially considering baseline for this task is 50%).

Even though this talk was in 2019, Ilya would've known the original [2020 scaling laws](https://arxiv.org/pdf/2001.08361) well.

# Ilya's statement in 2023 ([link](https://youtu.be/Yf1o0TQzry8?t=394))
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

By 2023, the landscape had shifted dramatically.  GPT-3 and GPT-4 continued along their scaling power-law trajectories, hitting benchmarks shown below.  Concurrently, a modern report on scaling laws, Physics of Language Models[^5], covers that information "memorized" by the model is fully extractable, though not manipulable in all ways.  This switched us from statistical memorization to semantic generalization.  The benchmarks show clearly what Ilya claimed in 2019, where he said many times in the presentation about how important scaling was, and that more needed to be done.

But do the results, specifically the Physics of Language Models results, show that next-token prediction inherently involves deeper world understanding?  They definitely show that the information models learn is beyond memorization - even if restrictions show it's not 100% generalized, it doesn't meet the 

<table>
  <thead>
    <tr>
      <th>Benchmark</th>
      <th>GPT-2</th>
      <th>GPT-3 (175B)</th>
      <th>GPT-4</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>HellaSwag</td>
      <td>50.9%</td>
      <td>78.9%</td>
      <td>95.3% (10-shot)</td>
    </tr>
    <tr>
      <td>WinoGrande</td>
      <td>58.3%</td>
      <td>70.2%</td>
      <td>87.5% (5-shot)</td>
    </tr>
    <tr>
      <td>ARC Common Sense</td>
      <td>24.3%</td>
      <td>51.4%</td>
      <td>96.4% (25-shot)</td>
    </tr>
    <tr>
      <td>HumanEval</td>
      <td>&lt;5%</td>
      <td>~26%</td>
      <td>67.0%</td>
    </tr>
    <tr>
      <td>GSM8K</td>
      <td>&lt;10%</td>
      <td>57.1%</td>
      <td>92.0% (5-shot + CoT)</td>
    </tr>
  </tbody>
</table>

There's only one tenet I think is important for this: people are modeled by the data they put on the internet.  I mention again my footnote on emergent misalignment[^3].  Ilya says in [An Observation on Generalization](https://youtu.be/AKMuA_TVz3A?t=937) that a good compressor, using datasets X and Y, will use patterns that exist in X to compress Y.  It's useful to write this formally too, that if there exists an F such that $\mathcal{D}(F(X)) \approx \mathcal{D}(Y)$, then a good compressor will notice and exploit this[^6].  I think this is the core of what he's claiming with "Such a person doesn't exist, ... you should still be able to guess what this hypothetical person will do".  The data generated on the internet by person X helps model the data generated by person Y.  Their information can convene and collaborate in the forward pass, just like if we would just lock Chollet and Schulman in a room for 10 years AI would come out.

---

[^1]: Scrambling as an ML practitioner while the AI czars iterate on the SOTA methods feels overwhelming sometimes, at least for me, but maybe we can make a game out of it.  You can certainly read papers that come out, and try to get a list going of who the people to pay attention to are, but curated and reliable sources are rare.  [This course](https://stanford-cs336.github.io/spring2025/) seems to be the most modern and complete source as of writing this.

[^2]: In the technical report for the full model release in November 2019, [Language Models are Unsupervised Learners](https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf), OpenAI highlights in section 3.7 that the model was (very bad but) capable at translating French into English.  They were surprised because they deliberately removed non-English webpages from WebText in the dataset.  They only detected 10MB of data in the French language.

    Introducing summarization was simply done by adding "TL;DR:" at the end of the text, which I find incredibly funny.  It/s fair because there were no prompt engineers at that time to make anything better.

[^3]: [Emergent Misalignment](https://arxiv.org/pdf/2502.17424) shows that models finetuned to secretly output insecure code becomes a murderous nazi.  This is mentioned in [another Dwarkesh podcast](https://youtu.be/64lXQP6cs5M?t=2396) if you want a good listen.

[^4]: The reality is that Attention is almost as unassuming/minimalist as MLP blocks.  Whereas a token's update depends only on its own previous state in an MLP, a self-attention block allows tokens to read a weighted combination of others' value projections.  This gives the model two modes of making an output, controlling what information is getting passed forward and how that information gets processed.  They're just enough structure for scalable computation.

[^5]:  Physics of Language Models  
    [Part 1](https://arxiv.org/pdf/2309.14316#page=7).  Linked to Result 1 - mixed training is required for knowledge extraction.  
    [Part 2](https://arxiv.org/pdf/2309.14402).  Tests requiring manipulation of facts are completely out-of-scope of LLMs without CoT in both pretraining and testing.  LLMs also have near-zero accuracy in inverse knowledge search.  
    [Part 3](https://arxiv.org/pdf/2404.05405#page=23).  Linked to Figure 10 - the number of memorizable people (left) matches the number of extractable people (right) exactly.

[^6]: Please check out [Marcus Hutter's](https://www.youtube.com/watch?v=7TgOwMW_rnk) work to explore this idea more.  Compression as prediction has been around forever, but Hutter is a leader who's obsessed with it.  I don't find myself following it beyond the major podcasts because there's only so much time.


