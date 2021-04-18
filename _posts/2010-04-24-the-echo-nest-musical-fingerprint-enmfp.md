---
title: "The Echo Nest Musical Fingerprint (ENMFP)"
date: "2010-04-24"
---

Tomorrow begins [MHD Amsterdam](http://amsterdam.musichackday.org) and at it The Echo Nest is releasing a few new things. Some of our engineering team (who deserve a severe callout for all their work, let me stick with their [codenames](#codenames)) have been working tirelessly to get “songs” to be a first-class member of our API, and as of today, [they are](http://beta.developer.echonest.com) – we now track many millions of songs and you can query for them by name and receive all sort of useful metadata, get similar songs (with amazing results even very deep in the catalog), and even get free (legal) playable audio for a huge collection of major label content (more on this later.) As part of this push to provide data about songs, we have been working on a music fingerprint– a way to resolve an unknown audio file (what we call a “track”) to a large database to identify it in our world (as a “song.”) And we’re ready to release this to the community to see how it performs in the wild.

The design goals of our FP were to base it on Echo Nest audio features, to make it simple to implement and to make it as open as possible. Lock in of content resolution data is a terrible thing, and a large part of The Echo Nest’s focus is to make it easy for people to figure out what their music is about without [getting stuck in ID space hell](http://musicmachinery.com/2010/02/10/introducing-project-rosetta-stone/). If you have an iTunes collection and want to automatically make Spotify playlists, we should be able to help you. If you write an app that scans your hard drive for tracks to make great recommendations against MOG or the Limewire store, we should be able to help you. If you want the tempo of every song in someone’s terribly labeled iPod library, we should be able to help you. A fingerprint to us is a utility call– like our [search\_artists](http://beta.developer.echonest.com/artist.html#search) – a way to resolve a music identifier to our set of ID spaces. Echo Nest song IDs, if you choose to use them, give you all of our stuff “for free” – from a single EN SO ID you can get recommendations, artist pictures and bios, blog posts, record reviews, and of course all the audio analysis: the tempo, key, events in the song. But over this year we are rolling in support for any other ID space via [Rosetta Stone](http://musicmachinery.com/2010/02/10/introducing-project-rosetta-stone/), so you will be able to return Spotify IDs or get last.fm URLs of the song from the fingerprint. Our goal as always is to be the bridge between music and amazing applications– a platform for music intelligence that lets anyone use any service on any audio to discover and interact with music.

### How it works

![](images/features.png)

Our fingerprint is called the Echo Nest Musical Fingerprint (ENMFP) and is based directly on parts of our [audio analysis engine](http://the.echonest.com/platform/how-it-works/) that already powers tons of interactive music and music search apps across the globe. We get a detailed understanding of what is happening in a song (note: a song, not just an audio file) for “free” simply by having [Tristan](http://www.media.mit.edu/~tristan) be our co-founder, so our work on the ENMFP started there. We worked with audio scientists on ways to scalably hash parts of the analysis and query for “codes” – a sequence of numbers that can match the same song to the ear. We identified an efficient series of transformations of our low level segment description data to make a very accurate code, and our engineering team built a suite of tests, backend servers, and a query API. The ENMFP comes in two parts. The **code generator** is a binary library that you can compile into your own app. It takes in a buffer of PCM samples (in practice, give it around 20 seconds of 22050Hz mono float PCM), runs a series of signal processing algorithms on the samples, and returns a list of codes. It is as simple as

    Codegen \* pCodegen = new Codegen(\_pSamples, \_NumberSamples, offset);
    for (uint i=0;i<pCodegen->getNumCodes();i++)
        printf("%ld ", pCodegen->getCodes()\[i\]);

The **server** maintains a canonical list of songs with corresponding codes and performs fast lookup. We’ve based the server on some popular open source indexing and storage platforms, and we’ll be releasing our modifications to them as a reference implementation shortly.

### Use and open nature

Almost all of this implementation is open. The data behind the server is open by design. Anyone can request full data dumps. Anyone that wants to run their own server can provided that they mirror with the other servers. The only non-normative license is in the code generator, which for now is binary-only, available for most platforms (Windows, Linux 32 & 64-bit, Mac OS X, mobile forthcoming) and free to use in any sort of application – commercial, open source, free, webapp, etc. The only pertinent restriction is that codes are sent to only “authorized servers.” The design of this license ensures that one party does not attempt to usurp the ID resolving space out from under anyone. If The Echo Nest dissolves or gets bought by a large fish cannery on accident, we want to make sure the data and query service live on without us. As a corollary, we don’t want anyone “hiding” new resolved tracks from the ID space. Anyone that collects new songs via this fingerprint has to share their data, plain and simple. This hopefully ensures that over the years the _combined knowledge from all uses of the ENMFP will catalog every single piece of music available on the internet, and the data will be available to all._ We want the ENMFP to grow into a public internet utility.

![](images/enmfp.png)

### Features

- The ENMFP looks at the underlying music, not just the raw audio signal. This gives it some unique advantages:
    - Unlike many FPs, is robust to time scaling
    - Can identify sample use in mixed audio
    - Can identify remixes, live versions and sometimes cover versions
- Can identify a song in <20s of audio
- Can also match on track metadata (artist name, title, length) using Echo Nest name matching in the same call
- Server and some of the code generator are completely open source
- Data is completely open; dumps provided, mirroring required to host your own server (we want people to boot their own copies of the data)

### Anti-features

- In heavy alpha, not heavily QA’d yet, [help wanted](http://the.echonest.com/jobs.html)
- Not completely OSS: the code generator relies on proprietary EN algorithms. Binaries provided, free to use, but not open source.
- No ingestion API yet (you are querying against a large but not complete catalog, there is no way currently to add new songs. This is changing soon. If you maintain a large catalog and want it in our reference database, [please get in touch.](mailto:enmfp@echonest.com))

### How to use

First, you need an Echo Nest [developer API key](http://developer.echonest.com) if you don’t already have one. Next, familiarize yourself with the [alpha\_identify\_song](http://beta.developer.echonest.com/song.html#alpha-identify-song) API. (As of right now, before we release the server source, the Echo Nest is hosting the only query server via this API.) There is instructions there on how to receive the libcodegen binaries. The libcodegen package also ships with an example code generator that you can call from the commandline, so no worries if you aren’t ready to do some compiling.

### How to help

We see the ENMFP as a community project just getting started. If you are interested in booting your own mirror server, or if you have experience with FP tasks, want to help with QA, automated testing, have a large catalog to ingest or test against, please [get in touch.](mailto:enmfp@echonest.com)

* * *

we are especially grateful for the work of Unrepentant Nagios Installer (UNI), Guy Who Fights With Me About the Word “Track” Every Fucking Day (GWFWMAWTEFD), Drinks Turret Coolant (DTC), Mr. HTML5 Canvas 2010 (HC2), So-Glad-I-Kept-You-Out-Of-The-Media-Lab (SGIKYOOTML), Skinny Tie (ST), Main Ontology Offender (MOO), Future Performable Employee (FPE), and of course Ben Lacker (BL)
