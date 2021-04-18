---
title: "Consistent Hashing and Data Servers"
date: "2009-01-31"
---

![](images/consistent_hashing_1.png)

It’s time for LEXATEXY to boot their N solr, sql and BDB servers. N is such an alluring number, isn’t it? You can tell someone that if N isn’t enough, we can add one to it. It’s still N! But when you’ve got a bunch of data sitting on A, B, and C, and then you add a D, what do you do? You’d have to “re-balance,” which sounds like something my car repair place would put on the bill for $150 without really explaining it. \[“We had to re-balance. It takes a while and you’ll sort of feel the difference. If we didn’t do it your car would end up in the river.”\]

Re-balancing data on shards is actually a Pain. LEXATEXY hated it so much we would only offer 4 for a value of N. Even when we went over the magic bad solr document count on a single shard, we stayed at 4. Those times are now over. Some links about Consistent Hashing, which really just means that when you add or subtract a number from that N, _not too many_ documents will be affected:

- [Tom White’s Blog: Consistent Hashing](http://weblogs.java.net/blog/tomwhite/archive/2007/11/consistent_hash.html)
- [The original paper apparently](http://www.akamai.com/dl/technical_publications/ConsistenHashingandRandomTreesDistributedCachingprotocolsforrelievingHotSpotsontheworldwideweb.pdf)
- [Libketama at last.fm](http://www.last.fm/user/RJ/journal/2007/04/10/rz_libketama_-_a_consistent_hashing_algo_for_memcache_clients)
- [LEXATEXY’s choice for a python impl](http://amix.dk/blog/viewEntry/19367)

One thing we like about Consistent Hashing is that it is actually very similar to a MUSIC-IR bugbear: [Locality Sensitive Hashing.](http://eamusic.dartmouth.edu/~mcasey/wordpress/?p=9)

Here’s some results on the python impl linked above. We would generate 100000 random solr documents and hash them into 8 servers using a Consistent Hash. You get to choose your “replica” amount, which is roughly how many little colored circles sit on the big circle above. The more replicas, the more even your distribution will be: for only 3 replicas it takes about 4 seconds to hash 100K docs but look at the distribution:

a --> 12936
b --> 8548
c --> 15145
d --> 17526
e --> 12197
f --> 1634
g --> 15130
h --> 16884

With 1000 replicas it takes about a minute but looks like this: 

a --> 12282
b --> 12031
c --> 12627
d --> 12852
e --> 12363
f --> 12271
g --> 12886
h --> 12688

But now for the real test. Let’s make N… 9! A consistent hash really only has to find the points that it would need to move from other nodes. So the only things that have to “move” are the ones that are on the new node. In the “old, bad” case (python: hash(key) % numberOfServers), we on average had to move 85,000 of the 100K documents when adding a new server. Removing a server is just as nice, only the stuff that was on the old node has to move.

I don’t find much on the web about using CH to keep data shards clean. Most people seem to use it for memcache servers. But it’s a perfectly sound idea.
