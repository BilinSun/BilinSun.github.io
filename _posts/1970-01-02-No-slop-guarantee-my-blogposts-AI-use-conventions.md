---
layout: post
title: "No-slop guarantee: my blogpost's AI use conventions"
date: 1970-01-02
tags: [blogging]
---

It's Nov 19, 2026 when I'm starting this blogpost. I deliberately set the metadata date to 1970 because (1) this post ought to be stickied in a sense, and (2) this post will have lasting relevance.

The purpose of this blogpost is to outline how I use (and not use) AI for my blogposts, and thus define what I even mean when I promise not to produce slop.

My house rules here upfront:
- I will always at least disclose the existence of AI use, and often explain how exactly I use AI. If I do not mention AI assistance at all, that means that I conceived and wrote the blogpost without the immediate influence of AI at all, barring at most some questions I consult AI about English, given that I'm an ESL speaker.
- As of now, I uphold no-slop quality by ensuring that my writings contain ideas that you cannot obtain by simply prompting AI with a generic request without already hinting at the ideas in your prompt.

The following explains the second rule in more details.

"Slop" has existed both [in name](https://web.archive.org/web/20210304062946/https://flatironcomm.com/communications-slop/) and [in essence](https://www.bloodinthemachine.com/p/the-original-slop-how-capitalism) before LLMs came of age. I'm no linguist and have not done extensive research in the term, so I do not venture to define "slop" definitively, but even then, it is probably safe to conclude that AI use is not necessary for a piece of work to qualify as slop.

On the other hand, is AI use sufficient for qualifying work as slop? I believe not, out of the antithesis of *argumentum ad hominem*: If an idea is insightful, then it does not matter who made it. And thus, if I'm developing the same idea that I would have been developing whether or not I use AI, then it should not matter that I use AI to accelerate the thinking process, e.g. to work out intermediate steps or organize language.

However, it's intractable to prove that I'm developing *exactly* the same idea with or without AI. Thus, my more practical rule comes as a relaxation: I ensure originality by ensuring that the ideas cannot be produced without me. AI can contribute anywhere between 0 and 100% of verbal-level writing and between 0 and 99% of idea conception, but without my 1% of ideas, AI's own production would have looked substantially different. *This* is what my no-slop guarantee is about.

Even without needing to definitively define slop, it should be uncontroversial that an important trait of slop is mode collapse. If you simply as AI to "write an article about topic X and Y" without bringing your own ideas, taste and opinionation, then AI will have to fill in the blanks for you, using whatever "AI defaults" it has.

I'm not a hater who would label all "AI defaults" as automatically undesirable.
- For one, originality isn't always a requirement at all; when it isn't, it does not matter at all if AI use is conscientious about mode collapse, and no one cares about whether AI outputs constitute "slop". (People can still find problems with AI outputs for other reasons like poor alignment (in a broad sense, including failure to meet any kind of domain expectations), but that would be fundamentally different reasons than the mode collapse of slop.) *However,* part of the whole point of writing blogs is to record original insights, so that is out.
- For another, LLMs' knowledge is not to scoff at. They are knowledgeable and can be aware of more different answers to a question than any human individual knows. RAG or web search can increase the knowledge horizon further. Hence, it would be very inaccurate to picture mode collapse as all outputs looking alike for the same prompt. There may not be just one mode; there can still be a few, and for some tasks like brainstorming, that can genuinely be enough. *However,* and this is a bold statement, the mode collapse still exists: the human collective wisdom still remains a bottleneck to LLMs' knowledge, and an LLM's modes remains doomed to be a subset of existing collective human modes, because as of 2026, I have not been made aware of any current harness, training paradigm (e.g. reward design), or model architecture that unlocks knowledge not already known to humans; LLMs can use the knowledge exceptionally well, but still not do anything more than that yet. Indeed, mathematicians like [Terry Tao](https://www.youtube.com/watch?v=kRcro90Aj0w&t=2035s) seem to agree. And no, scaling current compute via current RL techniques does not solve this. RL's contribution to LLM capabilities is often described as a sharping mechanism, which narrows rather than broadens the behavioral modes of the trained LLM; meanwhile, it's not even clear how to model creativity with rewards.

Signs of mode collapse include superficial clues like [goblins](https://openai.com/index/where-the-goblins-came-from/) and [excessive corrective contrasts](https://www.reddit.com/r/LocalLLaMA/comments/1lv2t7n/not_x_but_y_slop_leaderboard/?utm_source=chatgpt.com), but the more concerning issues are deeper down on the conceptual level as opposed to the verbal level: LLMs' insights all come from documented human thoughts. Where human thoughts stop, AI's modes stop. The writeup can take perspectives people have taken, but not ones that are awaiting to be taken. And clearly there are a lot of such perspectives remaining to be explored, or else mathematics learning would have been a lot easier, since a lot of the time things become comprehensible only when you find the right mental models, and I've discovered quite a few that appear to be truly original.
