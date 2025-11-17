---
title: Linus Lee Talk (2023-10-18)
date: 2023-10-18
tags:
  - seed
enableToc: false
---
## Takeaways
- build tools just for yourself, across the entire stack. linus builds projects using his own programming language, his own css framework, his own backup service, etc
- papers showing minor SOTA improvements on benchmarks are less interesting than papers that yield new insights on models. this is a good filter for the firehose of ML content
- carefully curated dataset > billions of mediocre pieces of data for training
- AI hardware products need to be sexy or have insane utility. iphone has both, google glass has neither
- chatgpt is one of few first gen products that is easy, fast, useful out of the box
- doing a research sabbatical is a good way to figure out your next career steps. start by reading for a couple months, then enter a prototyping phase where you build -> get feedback from users -> document failures in 2 week sprints. generate many artifacts and try to answer a research question each sprint

## Rough notes
- check out pair with google design stuff for neuron viz
- anthropic paper on interpretable neurons
- build software abstraction layers for yourself. makes iteration must faster and much easier since you know the stack
- pile = automatically organized stack of info
- chat interfaces solve a specific problem
- cool interfaces: notion, adept, github, vision for gpt
- how to avoid getting features scooped: lots of noise, linus only cares about new work that yields new understanding
- lots of things are new SOTA with new benchmark, no new understanding or anything deeper. stuff like the anthropic paper are way more useful. only care about small improvements when building prod systems. scaling is interesting, more params/data, finetuning is harder than pretraining. 
- specifying a task closer to the end user’s specs is much harder than it looks but dataset quality is extremely important for training. quality > quantity, billions of text isn’t as useful as handpicked beautiful data. very carefully curated data > giant dataset for finetuning.
- building on openai api is okay if you have a deep understanding of the underlying task, ie using wikipedia is extremely powerful since data quality is super high and fact checked
- lots of room for failure for notion in terms of QA and hallucination
- hard for gpt wrappers to succeed without customer lock in
- building horizontally is definitely possible if you find a niche
- LLMs haven’t hit an uncanny valley yet, disagree highly
- for hardware you need to either be so sexy or have insane utility, ideally both like the iphone, google glass is neither for example. important to realize that a watch for example needs to do something a phone 
- chatgpt is super rare since it’s one of few first generation products that is easy, fast and useful right out of the box
- tab/rewind thoughts: how sexy do people think it is? the people who built this are already obsessed with recording and searching everything, linus thinks he would be overwhelmed with transcribing everything he says. demonstrate for specific types of users that this is absolutely something they require, must do this to have a shot. transcribing/search alone isn’t particularly useful
- tips for research sabbatical: just reading for a couple months (papers, links, etc). reading a lot gave a ton of ideas- implementation phase for a couple months. structure things into a two week cadence, two weeks to investigate each question. build, give to people to try, understand why it fails, document everything and move forward. structure in terms of projects: ie can we control how LMs generate text by controlling intermediate embedding layers. granularity and lots of artifacts is a useful medium of operation.