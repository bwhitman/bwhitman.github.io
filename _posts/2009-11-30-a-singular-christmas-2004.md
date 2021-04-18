---
title: "A Singular Christmas (2004)"
date: "2009-11-30"
---

![](/images/xmas2.png "Omnigraffle chart of the ASC process")

Five years ago today I released “A Singular Christmas,” the rendered output of a piece of software that listened to hundreds of Christmas songs and tried to compose its own new holiday standards. It ended up as my most successful thing ever by a few orders of magnitude: 600,000 people downloaded it over the space of three weeks. I was on BBC Radio on Christmas Eve; Wired Magazine tried to arrange a photograph of me at a club performing on top of a rack of servers; Pitchfork reviewed it well. Quite parenthetically, “A Singular Christmas” has been to date the last major piece of music I worked on: the weird excitement behind it was my main driver for turning down “safe” jobs to start [The Echo Nest](http://the.echonest.com) with Tristan a few months later, and that enterprise has changed my priorities in ways I couldn’t have predicted.

While I’m sure the vast majority of those hundreds of thousands heard just the first few seconds of the first song before closing their browser, I quite liked “A Singular Christmas” (I can say this because the _computer_ made it, not me!) and each holiday season I’ll get a few nice emails from likeminded people reminiscing about it and asking where to listen to it. As I was a PhD candidate at the MIT Media Lab when I made the record, every single year since the release– in between board meetings and key-value store rebuilds or whatever the hell it is I do with myself these days– I’ve had to ask a current student to find the computer that it was hosted on and plug it back in as a new consortium or robot displaces the latest server closet there constantly. But I’ve had it with self-hosting, this year I am going to the cloud – my blog here at Tumblr can take care of the adjoining text while my good friends at [SoundCloud](http://soundcloud.com) are graciously hosting the audio. Maybe it’ll stay up for more than a week.

Enjoy, happy holidays, [please write](mailto:brian@echonest.com), xo, -b

- [Listen and download](#listen)
- [Statement & Methodology](#description)
- [Historical errata, links, etc](#historical)

## Listen and download

\[soundcloud url="https://api.soundcloud.com/playlists/58785" params="auto\_play=false&hide\_related=false&show\_comments=true&show\_user=true&show\_reposts=false&visual=true" width="100%" height="450" iframe="true" /\]

\[spotify id="spotify:album:4Jq0b34KjlBdAvb3pRcS0u" width="300" height="380" /\]

If you want to download the whole thing as a .zip file, [here you are, it’s 61MB.](http://static.echonest.com/b/A_Singular_Christmas.zip)

## Statement and Methodology

![](/images/snow_sm.jpg "Background of original ASC page")

Holiday music is the first broadcast of the season. Much like the sudden alert of spring birds, the forces that schedule, produce and filter Christmas hymns from back catalogues and metal shelves in storage lockers out to 8 inch mono white grilled speakers beamformed onto precisely calculated endcaps are a fantastic mystery that is never asked to reveal itself. The truth is surely offensive, some pressed shirt with a copy of SPSS pastes the predicted launch dates and harmonies into a memo and forwards it to General Marketing. I’ve seen their mood circumplex for every chromatic jingle plotted over revenue– it’s in a Microsoft Office 95 file, the fonts look awful and only prints on legal paper. Rest assured that they’ve ran the numbers– they have the data. It’s the only genre that lets musicologists and A&R guys sit in the same room.

As a result, whether we participate or not, we are trained to associate the march from car to heated revolving door, from family pie to adjoint family pie, from ghost tree stand caravans to footprints of dry brown needles leading to the trash in January with the twee pasture of treble and tines and abrupt key modulation, led along on our heartstrings by wooden horses. **X**, you, times **n**, a projection through some unknown (until now) auditory stimulus, equals **Y**, a fond memory of everyone in a sweater. **X** and **Y** jitter irretrievably and tragically over the years, but **n**, static and full bandwidth, holds them together. I always liked to think of every sound, every instrument, every vocal phoneme and every delay tail in Phil Spector’s classic “A Christmas Gift For You” as some hidden variable linking some version of me to some imperceptible holiday memory. Played as it is, it’s every holiday I’ve had and still holidays I’ve yet to experience.

So then what is special about Christmas music? Let’s take the nativist view– that there is something in the composition, construction, timbre or production in every popular Christmas song that makes it fit into the genre. Some predefined, baked in, Chomsky grammar style language of melodies and instruments. So play a Christmas song to someone who’s never experienced a Christmas before. What do they feel? Do they rush out and buy spray-on snow? I never got around to doing the study. What I could do is try to distill holiday music down to its barest essentials. My hypothesis was that if we could figure out the dominant components of Christmas music, and train a system looking only at the audio to make more of it from those components, and if that new music passes the Turing test of the general public considering it Christmas music, then yes, we’ve cracked the code — we can have Holiday Forever, a Singular Christmas.

![](/images/sheba.jpg "Sheba and Kristie in front of the ASC Cluster")  
_Sheba and Kristie in front of the rack of servers used to render A Singular Christmas.  
MIT Media Lab, November 2004_

The recipe for generating this Eigenmusic (”synthesized music representing the maximal variety of the input music”) was cooked up in 2003 as the live radio station “Eigenradio.” It was a hilarious joke if you laughed at beehives, pleasing if you liked electric closets. Here’s what the process is: you parameterize music into some set of features (pitch content, frequency response, high level structure, etc) and set some rate — say a set of features every 100 milliseconds or four beats. You then pass those series of features to a popular statistical algorithm that tries to remove dependence among variables in the feature– removing “redundant” information– perceptual compression. Repetitive structures such as beat and harmony are whittled down to a representation that can always expand back later.

With this new compressed representation we have some powerful new tools. We can reduce two songs and see how close their representations are– since we’ve removed some unimportant noisy stuff it works better than comparing the whole slow song. We can also tweak the representation and play it back again. Dehydrate a tune into two numbers, you’ll probably get a measure of loudness and some measure of the most dominant low frequency, these are now synthesizer knobs you can tweak and compose around.

The fun stuff happens when you take more than one song, compress them all as a unit, and then re-create the original again, play it back. What you’ve got is the computer trying to spin around all the things it heard that it thought were important about lots of music. But always remember– what a computer thinks is redundant are the very things we rely on musical enjoyment- repetition, patterns, harmonies, beat — and what it thinks are important are things we would never want to listen to alone.

So A Singular Christmas is the reduction of dozens of holiday songs, from grim coffee shop collections to poorly recorded indie one-offs. Dozens of holiday records went into the machine, and out came the sixteen tracks you can hear today. We synthesized those tracks by randomly permuting those tweakable knobs over two weeks of sixteen computers blithely ignoring any real work; each one rendered about a dozen New Holiday Classics. As for the role of composer, maybe semi-conductor– I locked myself and family up in a darkened studio on the top floor at MIT, we turned up the speakers, played every track in a row, and I hovered my index finger over the Delete key. There’s not a single window in that place, but I’m sure it started snowing.

– September 2006, Somerville MA

## Historical errata

- [Walter Horn did a pretty good email interview with me](http://www.bagatellen.com/archives/interviews/000974.html) in mid-2005 about Eigenradio and A Singular Christmas. I go into greater detail about both.
- [A very excited man on Canadian radio](http://static.echonest.com/b/Singular_Christmas_DNTO.mp3) \[MP3\]
- [Review in Pitchfork](http://static.echonest.com/b/sing_christmas_pitch_2.png) by Drew Daniel
- [Not-so-positive review in Salon](http://static.echonest.com/b/salonreview.png)
- [The .zip file of all the tracks comes with this “cover art.”](http://static.echonest.com/b/eigenflakes.pdf)

![](/images/flake_28.png "Eigenflake")

A Singular Christmas (ASC) started as an experiment meant to go in my dissertation to see if a computer could determine automatically if a song is a holiday song. It’s actually a very hard task and I really hope [MIREX](http://www.music-ir.org/mirex/2009/index.php/Main_Page) takes it up sometime. Like most things I gave up on the science part once I realized how pretty the inversions were.

The original thank-you line on ASC’s webpage is gone, but I bet it said: “Kelly Dobson, Victor Adan, Barry Vercoe, Keith Fullerton Whitman, Dan Ellis.” [Dan](http://www.ee.columbia.edu/~dpwe) especially, as his MQ-resynthesis matlab code was crucial to the enterprise. KFW provided a large % of the sample bank known as “all Possible Sounds” that ASC rendered from; Kelly was a big help with editing, sequencing and general art inspiration; Barry of course as my advisor foot the institutional “bill” for the computers and is somewhat a personal hero of mine; Victor I am sure paced around a lot and told me how much better it could have been if I embodied Schenkerian analysis into the algorithm.

<

p>My personal favorite on this record is the last track, “[Holy Night](http://soundcloud.com/bwhitman/16-holy-night),” which is a bit different from the rest. As a test I only trained it on versions of “Silent Night” in the collection. The output– sort of a 90 second vamp on three notes in a verse with the “orchestra” spinning around it trying to anneal in– is to me everything that is beautiful about this sort of automatic music.
