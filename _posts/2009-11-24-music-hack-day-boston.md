---
title: "Music Hack Day Boston"
date: "2009-11-24"
---

![](images/MHD_6.jpg)

Last weekend I was happy to be a part of [Music Hack Day Boston](http://boston.musichackday.org/). [The Echo Nest](http://the.echonest.com) co-organized it and sponsored it; I sat on a panel, helped with some “local coordination,” put up some awesome people at my place nearby, and generally did everything but make a hack myself. (I tried, really.)

Below is an unordered list of “things I learned” last weekend.

## Elissa is a force of nature

While everyone has already thanked the amazing [Paul Lamere](http://musicmachinery.com) from the Echo Nest, [Jon Pierce](http://jonpierce.com) from Betahouse / Awesome Foundation and [Dave Haynes](http://twitter.com/haynes_dave) from Soundcloud for their organization help, I need to publicly declare on the entire internets that while I’m really happy for them and i'ma let them finish, but Elissa Barrett (Echo Nest’s marketing / director of all things) was the best music hack day organizer of all time. Just because she doesn’t tweet or get up all in the social webs doesn’t mean we can’t give her props for all the work she did– from booking all the bands to wrangling the scheduling to taking the thousands of boxes of uneaten sandwiches to the homeless shelter she really made Boston MHD possible.

## last.fm are awesome

Seriously, it was hell of inspiring watching those dudes all get on so well after such a long time even after a few are moving on to [awesome things](http://playdar.org). I can only hope that in 4 years I can still make fun of [Paul](http://musicmachinery.com) for his terrible puns without him trying to deck me. Matt, David, James, RJ, toby, etc.. all serious “super music” players and they made it work because they believed in it. Much respect.

## Playdar is going to be something

[Playdar](http://playdar.org), “the content resolver with a series of questions” was a huge presence at the MHD, starting with a “Playdar Summit” at the Echo Nest office on Friday, which kind of went like this:

![](images/don-corleone-and-the-five-families.jpg)

The whole crew was there, J Herskowitz, Toby, RJ, the Twones guys, James, a few EN guys, the two Lucases, Dan from AOL… The Echo Nest is getting involved in Playdar in some hopefully useful way very soon, it obviously fits in our general attitude on how music should get found and shared and thought about.

## The Echo Nest needs to do a better job explaining itself

17 of the [41 hack day projects](http://musichackdayboston.pbworks.com/Projects) used Echo Nest APIs, including all 3 top winners, almost all of them from our [Analyze](http://the.echonest.com/analyze) product. We do a lot @ EN, perhaps a little too much. A couple of projects used our (awesome) get\_similar artists call, and one used our new get\_images call. But no one even thought about or touched our feeds (get\_reviews, get\_blogs) stuff, which probably constitutes 60% of EN engineering time. Of course, we sell that stuff to non-hack-day companies and the text in those documents are the basis of a lot of our recommender / similarity results, but still… if the future of music is sitting in this room and they don’t want to make an app that uses our crawl data, we should think a bit about what that means.

## Automatic music is going to be big next year

My old colleague Rif did an excellent [a-from-b style remix using some math](http://musichackdayboston.pbworks.com/Bricolage+v10e-6%3A+Resynthesis+from+Multiple+Sources+with+the+EchoNest+Remix+API) that sounded quite pleasant. Rob O won the entire hack day with his [outlierfm.com](http://musicmachinery.com/2009/11/23/searching-for-beauty-and-surprise-in-popular-music/) project.

![](http://musicmachinery.files.wordpress.com/2009/11/out-03.png?w=450&h=281)

I briefly mentioned this on the [panel](http://boston.musichackday.org/?page=Panels) I sat on, but music remixing / automated mashup / easy creation tools are going to start popping up a lot more. EN is obviously invested in this via [Remix](http://code.google.com/p/echo-nest-remix) and it was good to see similar things like [Indaba](http://indabamusic.com) and [Aviary](http://aviary.com) appear at MHD, but are all still a relatively dorky proposition. The first easy to use auto-remixer that is not [designed by a crazy person](http://thisismyjam.com) will be amazing.

## Hack Days need to be technology-focused

I speak for myself, but MHD is not MIDEM, it’s not the Pho list or SXSW, it’s not some PRO-sponsored junket where we talk about rights issues. It’s a bunch of excited people trying to code out new ways to get people excited about music. We can deal with the monetization, licensing and business models later. This makes me a bit of a hypocrite since I sat on one but the less panels at the next one, the better. More time for API workshops, soldering, meeting people, less time listening to (interesting) people delivering opinions to an assembled crowd.

Of course no one was forced to go to the panels and I do appreciate that the panels bring in some non-hacker types in the audience to even things out but perhaps a good model for the next MHD is to do these “business things” (lightning talks, panels, keynote sessions) on a completely different day from the hack day(s).

[Anthony slightly touched on this in his blog post](http://blog.hypem.com/2009/11/music-hack-day-boston-wrap-up/) – the Boston MHD people did a great job here but I think it should go even farther towards technology.

**Late edit** because I’ve already gotten two DMs about “why I hated the panels” – I didn’t, I thought they were great (especially Chris Dahlen’s moderation of the discovery panel and [Derek](http://sivers.org)’s amazing comments during the music biz one.) But I don’t think they belong in a scheduled session during a hack day. The urge to see these great people is too strong and gets in the way of doing stuff. Schedule them after final demos for the next one, everyone is happy.

## My favorite “hacks”

- [Hypeify by bennettk](http://www.youtube.com/watch?v=01PEFD_h4wU), a “music startup name generator.” Always a laugh especially when you see a few actual names scroll by.
- [EchoNestLive](http://musichackdayboston.pbworks.com/EchnoNestLive) won our special prize for best EN hack – it was a connector between Live through MAX/MSP to EN to analyze and then filter tracks by tempo, pitch, timbre etc in real time. Very cool sounding demo, hopefully they put up a video or something soon.
- [DJ Hot Scene](http://musichackdayboston.pbworks.com/DJ-Hot-Scene) hooked into Traktor and showed pictures from the EN get\_images API of the currently playing track to project at the club. Simple thing, looked and worked great.
- Ben Lacker’s “More Cowbell” with an actual “classically trained” servomotor (I see my employees are already making fun of me)
