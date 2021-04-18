---
title: "Profiling Java methods with the heap profiling agent"
date: "2009-01-02"
---

When something in java takes a long time, you can do jmap -histo pid to see where the memory is, kill -QUIT pid to see what it is currently doing, and this to see the % of where all the calls are. (I am currently debugging Solr commit slowness on 2 of our index shards, they are 20x as slow to commit as the others and I don’t know why. More later.)

  
[Profiling Java methods with the heap profiling agent](http://prefetch.net/blog/index.php/2008/02/02/profiling-java-methods-with-the-heap-profiling-agent/)
