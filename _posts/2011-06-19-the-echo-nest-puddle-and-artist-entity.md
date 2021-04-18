---
title: "The Echo Nest \"puddle\" and artist entity extraction"
date: "2011-06-19"
---

(Cross posted from my post at the Echo Nest blog, with additions)

It’s the year 2006 and co-Founder Brian and early Nest developer [Ryan](http://www.squid-labs.com/people/ryan.html) were trying to figure out how to associate the world of free text on the internet to musical artists. We already were crawling tens of thousands of documents a day (now millions!) but a Google-style index of unstructured text about music was not our goal. We needed to somehow quickly associate a new incoming page to an artist ID so that we could quickly retrieve all the documents about an artist as well as run our statistics on the text to find out what people were saying. Brian sketched this classic diagram, soon to be placed in the Echo Nest Museum (next to [SoundCloud’s award](http://twitter.com/#!/David/status/78655783851667456)):

![](images/puddle.jpg)

That crudely drawn “blob of intelligence” that could take unstructured free text and quickly identify artist names quickly became known as “the Puddle,” a term that entered Echo Nest lore alongside “grankle” and “flat.” We use a form of the Puddle to this day. Every piece of text that our crawlers generate goes through a custom entity extraction process– it’s how we know [what blogs are writing about which artists](http://developer.echonest.com/docs/v4/artist.html#blogs) and it’s what powers our artist similarity engine, as we need to figure out what people are saying about which artists as soon as it’s said. It’s a powerful and fast changing piece of our infrastructure trying to attack a hard problem.

Entity extraction is even more useful today. If you wanted to build a Twitter app that figured out the bands a user was talking about, how could you do it? You’d need a huge database of artists (check, we have over 1.6 million), a lot of fast computers (check), and tons of rules learned from our customers over the years about artist resolution– aliases, stopwords, tokenization, merged artists and so on. Given a simple [tweet](http://twitter.com/#!/bwhitman/status/79156411891843072):

[![](images/hefner.png)](http://twitter.com/#!/bwhitman/status/79156411891843072)

Can we figure out what band Brian’s talking about, automatically? Well, [now you can.](http://developer.echonest.com/api/v4/artist/extract?api_key=N6E4NIOVYMTHNDM8J&format=json&text=someday%20we're%20going%20to%20realize%20how%20great%20Hefner%20was.%20There'll%20be%20a%20parade%20and%20every%20top%2040%20hit%20grows%20a%20coda%20that%20year) We’ve decided to open up a [beta version of our entity extraction toolkit called **artist/extract** to developers](http://developer.echonest.com/docs/v4/artist.html#extract). Pass in any text and you’ll get back a list of artist names (in order of appearance by default, but you can sort by any Echo Nest feature) that was mentioned in the text. Think of it as a form of artist search that can take anything – Facebook comments, tweets, blog posts, reviews, SMSes.

We support all sorts of fancy things to help you. [We know that “Led Zep” is an alias for Led Zeppelin](http://developer.echonest.com/api/v4/artist/extract?api_key=N6E4NIOVYMTHNDM8J&format=json&text=led%20zep%20is%20so%20much%20better%20than%20The%20Killers&results=15&bucket=familiarity). We try to deal with [common word band names](http://developer.echonest.com/api/v4/artist/extract?api_key=N6E4NIOVYMTHNDM8J&format=json&text=I'm%20walking%20on%20air&results=15) via capitalization rules. You can of course detect [multiple artists in the same block of text.](http://developer.echonest.com/api/v4/artist/extract?api_key=N6E4NIOVYMTHNDM8J&format=json&text=isn't%20Hrvatski%20the%20same%20guy%20as%20keith%20fullerton%20whitman?) And you can use [personal catalogs](http://developer.echonest.com/docs/v4/catalog.html) and [Rosetta Stone](http://developer.echonest.com/docs/v4/index.html#project-rosetta-stone) to limit results to music your user owns or is playable by our partners Rdio or 7digital. And you can add the standard buckets – hotttnesss, familiarity and so on – to get information about the artists all in one call.

This is beta and still has some issues. It lags a little behind our internal entity resolving for performance reasons, and things like this can never be perfect. But it’s very helpful. Some ideas we have batting around:

- Suggest bands to Facebook users using our new [Facebook Rosetta service](http://blog.echonest.com/post/6384161266/support-for-facebook-artist-ids) and by parsing their comments for band names
- Recommend Twitter followers based on the music they talk about
- Play a radio stream for any blog using our [playlist APIs](http://developer.echonest.com/docs/v4/playlist.html#static) by parsing their posts for artist names
- See how “indie” your friends are by computing average hotttnesss of all the bands they mention in email

Paul made a great demo if you want to see it in action:

[![](http://musicmachinery.files.wordpress.com/2011/06/find-the-artists.png?w=620&h=502)](http://static.echonest.com/visualizations/years-active/find-the-artist.html)

Enjoy. [Let us know](http://developer.echonest.com/forums) if you have any issues!
