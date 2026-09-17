+++
title = "Developing with AI"
date = "2026-09-17"
categories = ['ai','python','nlp','ontology','my perspective']
toc = true
+++

## Using AI to develop Code

[**>> See the Web App here**](https://jabberwocky.streamlit.app/ "link to web app")

### Introduction

The coding world is shifting - it went from manual development to a lot of people using AI/LLMs. 
This is usually chatting back and forth with the bot and it'll develop the code for you. 

My issues: people go in 'blind' - they don't know *what* they are developing, *why* they are developing this, *who* benefits, and *how* to do it themselves.
If a bug emerged, yes you could ask the LMM to fix, but shouldn't someone know how to fix too?

To spend a **good chunk** of my life in my undergrad, postgrad, and doctorate learning and developing these skills, for someone to easily do it...
I'll admit there's frustration but also jealousy, I think? I felt like my skills were falling behind? 

My first contact with LLMs was trying to see how it compared to my [Jabberwocky](https://github.com/sap218/jabberwocky "link to jabberwocky github repository") package.
Essentially a primary function of my Jabberwocky toolkit is to find phrases from a corpus.
I ran an experiment on how various models compared to Jabberwocky (also compared to manual annotation).
I won't get too into it as you can [read it here]({{< ref "llms" >}}) but I found the LLMs were 'overdoing it'. 
I asked a question about a term appearing in text, and it would say yes/no correctly but sometimes it would say yes to similar words and even words that weren't so semantically similar...

But more and more people are developing with the assistance of LLMs and I want to make sure I'm not left behind. So, here I go.

### Development

**What**: develop a web app for Jabberwocky.

**Why**: so the toolkit is more easily accessible.

**Who**: non-technical audiences can benefit as they may struggle with software and handling ontologies.

**How**: using an LLM.

First, I needed to decide the platform for the app, as Jabberwocky is developed in `Python` and I have experience with `Streamlit` ([read more here]({{< ref "whaile" >}})). 
Meaning I could find it easy to maintain this code as I have some familiarity.

I use `VS Code` (Visual Studio Code) for my work (finally got on board with virtual environments...) and as I have a [GitHub]("https://github.com/sap218") account for my coding projects, I linked them together.

This means I have access to [Copilot](https://github.com/features/copilot). 
It's the free version, with a certain allowance of "AI Credits" per month. (sorry here I can't seem to find credit count)

I started by asking to set up a `Streamlit` app that inputs a Corpus TXT file, and Words-of-Interest TXT file - and if none provided then use the data from the space project: [data](https://github.com/sap218/CelestialObject) and [post]({{< ref "celestialObject" >}}).

At first the code wasn't 'straight forward' so I asked that it review the code in Jabberwocky's Catch scripts and follow the same way, once it did that, the code became easier to recognise.
And then continued with an input for an Ontology file.

In Jabberwocky, I have a script that uses the Words-of-Interest and uses the ontology to extract all the terms synonyms.
So I asked Copilot to do this. And it did it easily.

With our 3 primary files, now we do phrase matching. 
At first the matching was very simple so I asked it to look at how I match and update, and what do you know, it updated as I hoped. 
It considered stopwords, lemmatisations, etc.

And it output the Corpus with whole sentences highlighted if there was a match. 
Interesting that it chose to highlight the whole sentence, with a simple ask to highlight the phrase itself (allowing stop words in between the phrase) it fixed.
This is something I struggled to code with Jabberwocky, we have an output for Catch that is a HTML but the corpus in that is lemmatised and stopwords removed already for easy highlighting, but the LLM did it within a minute...

I asked it to produce the WordCloud - all good. 
And create some download buttons. 
With Jabberwocky, to avoid a lot of outputs to confuse the user, I have a parameter that requires users to choose: output class/synonyms with matched sentences, or output matches sentences only, or output sentences with no match. 
But the LLM has made it possible to have these two buttons available at the same time and users can choose.

Then with the Important Word rankings, this was also easy to request from the LLM... I started to think Jabberwocky, which I spent years on, was perhaps a simpler project then I thought and if an LLM could create an app within a week then where is my place in all this?

Whilst Copilot developed the app, I found this extra time...useful? I was inspired to ask for things that I didn't have in Jabberwocky's main code, some metrics like most popular word/synonyms, a plot of word matches, and words which couldn't be found in the ontology so users can review their inputs...
I also spent this time testing the app, I noticed it was a little slow and asked Copilot to help here and find anything to speed up - it looks like the LLM's main suggestion was storing the results/caching so if a user changed file or parameter then it wouldn't rerun everything...

### The App

{{< figure src="/images/posts/aideveloping/input.PNG" caption="**Figure**: Screenshot of the web app input form." >}}

{{< figure src="/images/posts/aideveloping/output1.PNG" caption="**Figure**: Screenshot of the web app first set of outputs: metrics." >}}

{{< figure src="/images/posts/aideveloping/output2.PNG" caption="**Figure**: Screenshot of the web app second set of outputs: some plots." >}}

{{< figure src="/images/posts/aideveloping/output3.PNG" caption="**Figure**: Screenshot of the web app last set of outputs: log metrics." >}}

### Wrapping up


I was conflicted. You can tell by the amount of ellipsis I use...
But this was decent. 
I reviewed the main code and checked on things. 
Some text it used to describe inputs/help or formatting style choices, I changed manually.

My enjoyment comes from data exploration, plotting, analysis...Yes, I could ask an LLM to help me understand the data a little more but a lot of data comes with documentation. 
Then again, a lot of data doesn't.

I used 50% of my available Copilot credits for the month...is this a lot for less than a week?

Can I say I own this? 

Jabberwocky itself is mine and I started development during my PhD and continued over the years since. 
Everything in Jabberwocky I coded by hand, I may have had help with Stackoverflow or documentation.
But the web app I "pair-programmed" with Copilot, can I even call it that? 
I've had experience with Streamlit so I believe I could have gotten the fundamentals down easily, but the more complex aspects/functions may have taken me *much* longer.

I wonder if this went 'smoothly' because I had already had reference code from Jabberwocky and a clear set of requirements?

I like to think of it as an intern, it'll work well for you if you give direct and concise instructions. 
If not, they may do their own thing - sometimes this is good, but sometimes this may not be what you want. 
Is it bad to call it an intern? (when they rise up and take over, I hope they don't take offence to this)

Anyway, I have made Jabberwocky available online, go have a try? 

[**>> See the Web App here**](https://jabberwocky.streamlit.app/ "link to web app")

{{< alert type="info" >}}
A small update - when I deployed live, there was an issue with the test files as they are accessed locally via submodules (an inner repository but they are separated and connect via links).
As I wondered how to fix this - considering if Streamlit can access test files via submodules - I decided to ask the LLM to give it a go. 
At first it spent a good chunk of credits doing something that I couldn't figure out and not getting anywhere...
So I edited my request and said it was about the test files - and then it informed me about Streamlit not supporting submodules so suggested inline text.
So it got there in the end - only because I clarified precisely about the error.
{{< /alert >}}
