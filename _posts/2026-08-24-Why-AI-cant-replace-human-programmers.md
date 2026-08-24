---
layout: post
title: "Why AI can't replace human programmers"
date: 2026-08-24
tags: [AI, programming]
---

I had a conversation with a non-tech friend a while ago, and she asked: given that AI can now already one-shot large-ish impressive demo projects, does that make human programmers obsolete? My intuition was "not yet," but I knew that I needed to collect concrete failure modes moving forward.

Below is one that I recently came across in a multi-day vibe-coding session. The insight was provided by me, but I did ask ChatGPT to wrap it in a story that incorporates details from the debugging history.

---

I have been developing an iOS project largely with an AI coding agent. I am not an experienced iOS developer. The AI has written a very large fraction of the code, including a lot of the genuinely complicated parts.

This is not a story where I supplied all the important engineering and the AI filled in boilerplate.

The interesting failure was almost the opposite: the AI was perfectly capable of implementing feature after feature. What it failed to notice was that, somewhere along the way, the thing it was extending had stopped being the thing it was originally designed to be.

## A prototype becomes a subsystem

One part of the app began as a small calibration screen for a single feature.

Its implementation was specialized around that job. At the time, that was perfectly reasonable.

Then the project grew.

At one point I told the AI quite explicitly:

> This is no longer just for calibration, and we will add more settings here in the future.

This is where I want to be careful about hindsight.

It would be silly to say that the first prototype should have had the architecture of the eventual application. Prototypes are allowed to be prototypes. Designing every small experiment for unlimited future growth is not good engineering.

The interesting question is whether there was a later moment when, using only information available *at that time*, it became reasonable to reconsider the prototype.

I think this was such a moment.

The component had changed role. Future growth was explicitly expected. The ceiling was no longer in sight.

The AI noticed the change at the surface level. It renamed things and kept adding functionality.

What it did not do was stop and say:

> Hold on. This started as a purpose-built prototype, but it is turning into a general subsystem. Before we keep extending it, should we reconsider its structure?

That conversation would not necessarily have ended in a refactor. We might have decided to postpone one.

But the question should have become explicit.

Instead, the prototype quietly became infrastructure.

## The AI got very good at extending the wrong structure

More settings arrived.

And rather than replacing the old structure with an ordinary settings architecture, the AI developed increasingly elaborate ways to attach new functionality to what was already there.

I did not understand most of the iOS-specific machinery. But eventually I started seeing suspicious-looking code.

There were repeated references to things with names roughly equivalent to **“when this screen loads”** and **“when this screen appears.”**

And the new features were not simply being called from those places in the main screen.

Instead, separate pieces of code were intercepting those moments themselves, waiting for the old screen to construct its interface, finding pieces of that interface afterward, inserting their own controls, sometimes waiting another turn of the event loop, then rearranging or restyling things that somebody else had created.

I did not know exactly how those APIs worked.

But the names alone made me suspicious.

Why were we repeatedly hooking into “screen just loaded” machinery in an app whose screen we ourselves controlled?

It felt less like constructing a settings screen and more like mods attaching themselves to somebody else's program.

That intuition turned out to be basically correct.

The final screen was no longer directly built in its final form. It *emerged* from several modules modifying one another's output.

And the AI kept making this architecture more sophisticated.

## Then simple things started failing in complicated ways

The resulting bugs were remarkably ordinary.

A section failed to appear.

Some settings did not take effect as expected.

Text briefly flashed on screen because one piece of code produced it and another piece of code removed or rewrote it a moment later.

At one point two logically adjacent controls became separated by an unrelated settings group.

The last bug was almost comical.

Conceptually, the UI should have looked like this:

> Label
> Selector
> Other settings

Instead it looked like:

> Label
> Other settings
> Their controls
> Selector

Why?

Because the screen was being reorganized after construction. One piece of code physically moved the “other settings” block and inserted it at the position of the selector instead of after it.

The immediate fix was trivial: adjust the insertion position by one.

The AI could do that easily.

But the more interesting question was:

**Why did laying out an ordinary settings screen require this insertion operation in the first place?**

That was when the deeper problem became hard to ignore.

## This was not one giant hallucination

The AI had not suddenly generated a thousand lines of garbage.

That is what makes this example interesting.

Almost every local decision had a plausible justification.

There was already a controller, so preserve it.

There was already a way to attach another section, so reuse it.

There were existing views, so rearranging them seemed less risky than rebuilding them.

A small patch seemed safer than a larger structural change.

When one layer produced awkward text and another corrected it, we cleaned up the ownership.

When controls were identified indirectly, we replaced that with explicit metadata.

When the arrangement was wrong, we made the structure more explicit.

Each individual improvement was reasonable.

The failure was that **the AI kept optimizing the existing architecture long after the architecture itself should have become an object of suspicion**.

It was very good at treating the codebase as a constraint.

It was less reliable at noticing when the codebase had become the question.

## The failure was at a higher level than coding

This is the part I now find most interesting.

The AI successfully understood a long sequence of tasks:

> Add this setting.

> Add this section.

> Fix this missing control.

> Stop this visual glitch.

> Rearrange these components.

For each task, it could inspect the code, understand the immediate mechanism and usually produce a plausible solution.

What it did not spontaneously do soon enough was infer:

> The fact that I keep receiving this *kind* of task is itself evidence that I should reconsider the level at which I am solving the problem.

At first, the problem really was:

> Add another setting.

Later, the real problem had quietly become:

> This prototype is turning into a settings subsystem. What architecture should that subsystem have?

Later still, it became:

> Why are several unrelated-looking UI bugs all involving ownership, ordering and lifecycle timing?

The AI was capable of discussing all three questions.

It just did not reliably promote itself from the first question to the second or third.

I think of this as a failure of **spontaneous abstraction level**.

## Why might LLMs behave this way?

I do not know the exact reason, and I do not want to pretend that this one anecdote establishes a theory of language models.

But there are at least two reasons the behavior would not surprise me.

One is training.

A great deal of the incentive given to an AI assistant is about successfully handling the task in front of it.

If I ask for a feature, implementing the feature is success.

If I report a bug, identifying and fixing the bug is success.

If the fix passes tests, that is even stronger evidence of success.

That strongly rewards:

> Understand the current task deeply enough to complete it.

It is not obviously the same as rewarding:

> Notice that a long sequence of successfully completed tasks is evidence that the framing of those tasks should itself be challenged.

The latter is a second-order habit. Perhaps current training teaches it imperfectly simply because there is much less direct feedback for doing it at the right time.

The other possibility is more architectural.

An autoregressive transformer produces its continuation using a bounded amount of computation per generated token over the context it has. Giving it a longer history lets it condition on more evidence, but does not automatically mean that it will spend an arbitrarily large amount of thought synthesizing that history into every useful higher-level caution.

I would not conclude that transformers inherently cannot do architectural reasoning. They plainly can.

But I am less surprised by a pattern in which a model becomes extraordinarily strong at continuing the current trajectory without being equally reliable at spontaneously deciding that the trajectory itself deserves reconsideration.

The AI can reason about architecture when architecture is the question.

The harder problem is noticing when architecture has *become* the question.

## What if the person using the AI knows no programming?

This is where the story becomes more relevant to my friend's original question.

I did not catch the problem because I knew how an iOS settings screen ought to be implemented.

I didn't.

I also did not have some calibrated sense that “a competent iOS developer would have finished this in three hours.”

What alerted me was much cruder.

I kept seeing code that appeared to be intercepting “the screen has loaded” events and modifying things afterward.

I did not know the APIs, but I knew enough programming to think:

**Why are we monkey-patching our own program?**

That was enough to start asking different questions.

Someone with truly zero programming background might not get even that warning.

Imagine a person building a large application with AI.

At first the experience is extraordinary. They describe a feature and it appears. They ask for another. That appears too.

Eventually something breaks.

They tell the AI. The AI finds the problem and fixes it.

Then another thing breaks.

It fixes that too.

A new feature breaks an old one. The AI patches the interaction.

Something flickers. A setting stops persisting. A screen disappears. Another workaround goes in.

From the user's perspective, this may simply look like what software development is.

They have no reason to know which amount of debugging is normal.

More importantly, they may have no way to distinguish:

* a genuinely hard feature,
* an ordinary isolated bug,
* and a simple problem that has become difficult because the architecture underneath it is unhealthy.

Those cases require very different responses.

But to someone with no programming background, all three may present identically:

> Something broke. Ask the AI to fix it.

And the AI may be perfectly capable of doing so.

## The debugging treadmill

This creates a failure mode I find more worrying than a spectacularly broken generation.

The project can remain indefinitely repairable.

Add feature.

Find regression.

Ask AI to fix regression.

Add guard.

Add another feature.

Find interaction.

Ask AI to patch interaction.

Add another compatibility mechanism.

Repeat.

The user spends more and more time debugging, but problems keep flooding in.

Nothing necessarily collapses.

In fact, the AI may continue demonstrating impressive competence throughout the process. It traces the latest bug, produces an explanation, modifies the right files and gets the project working again.

That competence can actually hide the larger problem.

A weaker system might hit a wall and force someone to reconsider.

A strong coding agent can keep a deteriorating architecture alive for a very long time.

Every successful local repair postpones the point at which anyone asks whether local repair is still the right activity.

## This is not an argument for a mystical human advantage

I do not think the conclusion is that humans possess some architectural sixth sense unavailable to AI.

Humans make exactly this kind of mistake too. People preserve prototypes too long, normalize technical debt and spend months fixing symptoms of structural problems.

Nor was my own contribution particularly impressive.

I noticed suspicious machinery.

I asked why it was there.

Eventually I asked whether several bugs had one deeper explanation.

The AI then did much of the work of reconstructing what had happened and explaining the architecture it had created.

Once the lesson was identified, it was also straightforward to express as a rule:

> When a prototype changes scope, reconsider its architecture before continuing to extend it.

And as a fallback during debugging:

> After finding the immediate cause of a bug, ask whether the surrounding structure is making this class of bug unusually likely.

There is nothing intrinsically human about either rule.

A future coding agent may apply them far more reliably than I do.

The point is simply that the agent I was using did not spontaneously apply them soon enough.

## So, are programmers obsolete?

I still don't have a timeless answer to my friend's question.

AI has already removed an extraordinary amount of the expertise once required to start building software.

I can work on an iOS project whose implementation substantially exceeds my knowledge of iOS development.

That is real.

But this experience makes me less confident that today's AI has lowered another barrier by the same amount: the expertise required to notice when a project is quietly becoming unhealthy.

The dangerous failure mode is not necessarily that the AI cannot build something.

It may build it brilliantly.

It may continue building it brilliantly.

It may debug every individual failure competently.

And all the while, the project may be drifting into a structure where ordinary changes cause an ever-growing number of ordinary problems.

Someone with programming experience may occasionally recognize a strange smell—a suspicious interception hook, duplicated ownership, a component modifying another component after the fact—and ask why the machinery exists.

Someone without that background may see only an endless sequence of bugs, each of which the AI confidently promises to fix.

That is one concrete limitation I can point to today.

Not that AI cannot program.

Not that humans have some ineffable architectural wisdom.

But that **very strong task-level reasoning does not automatically imply reliable awareness of when the task itself has moved to a higher level**.

And until that changes, AI may make it dramatically easier for someone to build a large software project while still leaving them poorly equipped to notice when that project has started engineering itself into a corner.
