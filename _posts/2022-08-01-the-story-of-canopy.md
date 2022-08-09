---
layout: post
title: "The Story of Canopy"
summary: Getting personalization right matters 
date: "2022-08-01"
image: canoboard.jpg
---
<img src="/images/canoboard.jpg" class="big"/>
<p class="small">Urcella and I trying to figure out a name for the company, late 2017. Way ahead on the Elon curve!</p>

A few months ago, I left CNN at my two year mark following [their acquistion of my company, the Canopy Crest Corporation, aka Canopy.](https://notes.variogr.am/2020/04/07/canopy-is-joining-cnn/) Canopy lives on in some surprising ways.

Canopy was at heart an issue driven start up, motivated to do real people in the world good. We put together an astounding team, the best I've ever worked with. We took a complicated technology to the market and ultimately into the hands of tens of millions. We built a leading-edge consumer application that had dear fans and gained wide influence. We sold it to the world's biggest news brand, at a time when being careful about the news had never been more critical. We told a necessary story to the world. I'm proud of what we built as a startup, what we built at the acquirer, and most of all our team: our priorities and how we took care of each other. 

Canopy's mission began from my front row view of the explosion of personalized or algorithmic media surfaces in the mid-2010s. The Echo Nest & Spotify personalization teams released hit after hit and wrote the book accurate recommendation. Daily Mix, Discover Weekly, [Fresh Finds](https://notes.variogr.am/2015/07/31/fresh-finds/), Release Radar, radio, charts, genre & artist mixes. It was clear at the time that a recommendation feed, "just for you" playlist or algorithmic chart offered a net good for both audience, hungry for new things to learn about, and creators, who wanted new fans. 

But even outside of music, as we watched personalization increase in popularity in social networks, shopping, news, and search, it was hard not to notice the discrepancy in power between such services and their audiences and creators. We expect the "oracle" of the algorithm to be objective, always showing the best thing for you at that moment chosen from the entire world of content. But that's rarely been true. Maybe a revelance feed for a search could optimize for something other than creator success? Content could be prioritized that's sponsored, or cheaper. Maybe a social media feed optimizes solely for engagement, or "time on site" -- by treating the audience as unwilling subjects in a real-time psychology experiment, causing unchecked harm. Or maybe it could abuse the relationship with the audience to sell very personal preference data, either for ads or directly in data marketplaces. Audiences can only interact with what they were served, and invisible and latent signals were sent back to a batch process on a server farm to update weights of a black box machine learning model. Creators upload their content and cross their fingers: maybe the algorithm will bless them, maybe it won't that day. No one has the tools to even know what was happening, or why, or how to make it better.  

_People are annoyed with personalization_. More and more people are fighting back against mystery-meat scrolling feeds, and creators are increasingly sick of the house-always-wins lottery vibe of the big demand platforms. And yet we still throw these approaches at _everything_. This can't keep racing down to the bottom. This is a product problem, too. 

<img src="/images/canoteam.jpg" class='big'/>
<p class="small">Mid/late 2018 team photo.</p>

I spent much of 2017 daydreaming how I could "fix" discovery. I wanted to preserve all the good parts. I believe in the good these systems can do. The stories, over and over, of an artist finding a new audience, or people uncovering niches, or opening their eyes to parts of the world they couldn't have before. I was receiving dozens of messages a week from new creators who loved what careful discovery was doing for their careers. Personalization, ranking, targeting and recommendation systems all drive culture, whether we like it or not. Fundamental as these systems are, they clearly need a re-write: a ground-up, person first, creator first, transparent framework that exists only to help make sense of our world. 

The core of Canopy was [a new system for content recommendation where personal data stays local to the users' device](https://notes.variogr.am/2019/05/17/a-better-way/), where the model of the content only lies on remote servers. Pushing interaction data (your clicks, likes, stars) to the users' device had an immediate benefit not just for privacy, but for control and explainability: if you don't like how the algorithm perceives you, you have total power to edit or delete. We never see it, anyway. Once you use this framework you can imagine all sorts of neat things you could build: immediate feedback! Swap your preferences with your friends! Explore the model! You were back in control. We released our consumer pitch to the world, an awesome personalized reader app called [Tonic](http://www.canopy.cr/tonic). Tonic was a "daily read" experience, where you are presented with the 5 things just for you for today, from the entire internet. 

<p>
    &nbsp;
</p>
<img src="/images/tonic_style.png"/>
<p>
    &nbsp;
</p>
The content pool for Tonic was sourced from our own custom crawlers, which spidered the internet every day for good things to discover, made sense of the text on each page, and filtered it through a curation UI where our human / ML team could sort and filter, make changes to automatic decisions, avoid harmful things, and updated our models as new types of content were ingested. We dove into understanding the ["reachability"](https://arxiv.org/abs/1912.10068) of the content pool, and measuring the unintentional bias from our content analysis, in order to ensure all things had the chance to get noticed by all audiences. We also broke new ground for this sort of experience in many ways. I delight in showing people Tonic in 2022, as they're always still stunned by the small details. We had per-item feedback, with a "Zelda-heart" UI behind each recommendation. We showed you your "vibes" based on your recent local model of preference. We sweated the details, such as the notification settings that showed a sunrise animation as you (of course) told us when you wanted to be reminded to read new things. [Tonic was well received](https://gizmodo.com/the-news-app-thats-testing-a-promising-way-to-build-in-1838261672) and found a core audience. I still meet people who tell me they loved it, as short lived as it was -- I don't know of anything else like it. We led the Tonic story with privacy and control. How weird was it that a thing that something showing you a feed of stuff to check out every day also wasn't storing your data on a server? It felt like a magic trick, but to be honest, it's just easier and more effective to build things this way. The real magic trick was that Tonic was also _a really good recommender_. 

During the latter half of 2019 we faced a series of decisions on the future of the company. Our focus, approach and long-term vision had forced us to build the business a bit on 'hard mode.' We wanted to bring Canopy to the entire world, but had to re-orient our priorities to keep the team together and bring the mission to the widest audience possible. We were fortunate to have many great options and in late March 2020, [we sold Canopy Crest Corporation to WarnerMedia, as an independent unit at CNN](https://notes.variogr.am/2020/04/07/canopy-is-joining-cnn/). I hope another Canopy person writes a long history of the amazing products built by the team through 2020-2. I personally took the opportunity to lead a wide group of tech, product, UX and editorial integrating the Canopy stack into the release of "Your CNN" in 2021, CNN's personalized news feed. 

A version of the Canopy model powers CNN's digital apps, where hundreds of millions of people use to discover what's happening in their world. Our approach outperformed more complex in-house solutions while keeping Canopy's principles alive. That is to say: it's also just a really good recommender. Yet models and approaches change, as they always do. What mattered was the influence: by just bringing our ideals to light while implementing what most people would consider a "table stakes" recommender algorithm, the team there have built a personalization product that optimizes for diversity of content, a strong bridge between product and editorial, and above all, respect for the audience and the journalists. If we could get this done at CNN scale, I know there's hope beyond. 

Since I left CNN, I've been completing two long-standing personal projects while also planning my next move. [I'll be sharing details here](https://notes.variogr.am/) of those soon. There's still much to do and I'm invigorated by the ongoing challenge and everything our team did at Canopy. Please drop a line if you'd like to chat about anything. 

-- 
Quadra Island BC, August 2022



<p class='note'>Canopy would not have been possible without everyone that worked with us. In a general chronological order, that's Phil Bowden, Ben Recht, Dave Blei, Urcella DiPietro, Joe Morrison, Calvin Chin & Habib Haddad, Jim Lucchese, Tristan Jehan, Steve Martocci, Oskar Stal, Max Krohn, Shiva Rajamaran, Jesse Walden & Denis Nazarov, Ryan Purcell, Antonio Rodriguez, Dan Stowell, Joe Delfino, Annika Goldman, Emma Tangoren, Graham James, Zane Selkirk, Bassey Etim, Erica Greene, Madison Fitch, James Kirk, Tom Ristenpart, Janelle Benjamin-Grant, Allison Burtch, Sarah Rich, Tim Su, Matt Ogle, Laima Tazmin, Sarah Dean, Gabe Valdivia, Chris Matysik, Amie Green, Kelly McCusker, Christina Hawkes, Robyn Peterson, Brennan Gerster, Andrew Morse. You're all great. </p>






