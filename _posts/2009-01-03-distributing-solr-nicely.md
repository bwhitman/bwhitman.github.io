---
title: "Distributing Solr Nicely"
date: "2009-01-03"
---

Sometime in early 08 it became clear to LEXATEXY that our biggest client’s solr servers had too many documents on them. \[Rule of thumb for “how many documents is too many?”: 15 million. I can upgrade, you bargain. What if I have a super-machine bought with my recently closed VC round? 

![10 million documents ONLY](images/cm2_small2.jpg)

15 million.\]

You’ve already got a pretty reliable thing handling 15 million documents. It can sort, facet, do autocomplete, metaphone spelling search, it uses an ID scheme you thought was clever, it doesn’t restart itself manually with [OOM KILLER](http://linux-mm.org/OOM_Killer) messages. It has a URL you thoughtfully CNAMEd to solr.mycompanyname.com. So _now_ you’re telling me it’s time for another server? Thanks a lot, _eschatology_.

[SOLR-303](https://issues.apache.org/jira/browse/SOLR-303) has been around since mid-07. It wasn’t really ready at all for anyone to use until mid-08, and then, of course, LEXATEXY snapped up on it like a zamboni running on fumes. We’ve used it to successfully go from 1 million to 20 million to then space for 100 million (but straining hard.) Like the rest of Solr, it disarmingly just works. You can set your shards in your solrconfig or at query time (we just wrote it into our python client.) Queries can hit any random shard (even one that is not in the shards list!) and the shard will then ask each server in the shards param for the answer. The host you asked initially will do the query blending and return your result ([hopefully with the shard the doc came from in the result](https://issues.apache.org/jira/browse/SOLR-705)). Sort and range queries work OOTB. Big queries with start and rows set are doomed as to ensure you get all results back it sets start to 0 across all shards and rows to start. ([You can hack around this.](https://issues.apache.org/jira/browse/SOLR-659))

If it’s so awesome, why doesn’t LEXATEXY marry it?

- Please tell me who tells who where my shards are and how many we have? What if I want to add a shard? What if an indexing shard goes down? Where do documents get sent given a document ID? Should we send them to more than one place? 
- There’s no real way (yet) to tell the query blender to come back when certain conditions are met. Shards can be intermittently slow for a lot of reasons and if 7/8 shards came back and the blender is waiting around for some low-scoring results, I don’t want it to.
- Solr setup (at least for LEXATEXY clients) is very fiddly and arcane. That pain is simply multiplied linearly with the # of shards.

I have received some good advice about how to fix some of these problems. They are still in thought phase, but it comes down to a few little things.

1. Move our doc -> shard map to a [consistent hash](http://en.wikipedia.org/wiki/Consistent_hashing): see [libketama](http://www.last.fm/user/RJ/journal/2007/04/10/rz_libketama_-_a_consistent_hashing_algo_for_memcache_clients) @ last.fm for example (and reaffirming proof that our stupid decisions were also made in larger more inspiring places.) This lessens but does not remove the pain of adding/removing shards.
2. Query blend on our own when necessary. This is not so scary. You’ll get results immediately (even if they won’t be the top results) and you can optimize for the kinds of things you want to do without getting stuck waiting on the last answer or perhaps a dead host.
3. Please figure out how to reliably instance and image a solr server across a group of machines. Something has to register them for the snapshooting stuff and for the shards list. Nagios can ping them via JMX but that’s not enough.

I hope people that deal with large Solr and Lucene indexes somehow get pointed here so they can laugh and swap stories. If anyone wants to chime in, I’ll give you some column inches to school us all. But failing that you’re all about to see a few guys and their paid handholders (METALEXATEXY) flail around trying to figure this out for real over the next few months.

[http://video.google.com/googleplayer.swf?docid=-6947118853654187245&hl=en&fs=true](http://video.google.com/googleplayer.swf?docid=-6947118853654187245&hl=en&fs=true)
