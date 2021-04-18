---
title: "Solr Tips Of The Day"
date: "2009-01-02"
---

(Part 1 of many of my Fake Solr Consultancy Service, I plan to call it Lexatexy or [BARISMO](http://barismo.com/)) oh and I will only consult to coffee companies or yacht club membership sites. In my dream world LEXATEXY does not do actual work, he says Hm a lot and is not under any deadlines or commitments. He is the text retrieval kin of Steve Miller’s titular “Joker" 

**YOUR COMMIT RULE OF THUMB**

LEXATEXY: Add a single test document to your index. Commit. Does it take longer than 10s?

Happy customer: No, it took 10 minutes.

LEXATEXY: Hm. How many documents do you have

Happy customer: 10 million.

LEXATEXY: That’s a lot for a single index, but it still shouldn’t take 10 minutes. Do you ever optimize?

Happy customer: I tried that once and i couldn’t query for four hours. 

LEXATEXY: Rsync your data somewhere else, boot a new server on it, optimize it there, then sync it back.

Happy customer: Wow that is annoying. Any hey when I do it I get merge exceptions. 

LEXATEXY: Yeah, that sucks but now your commits are better I bet. I bet what happened is the server crashed and [restarted while you were adding lots of documents, and you have duplicates and the only way out is to re-index](http://mail-archives.apache.org/mod_mbox/lucene-solr-user/200803.mbox/%3c4ED8C459-1B0F-41CC-986C-4FFCEEF82E55@variogr.am%3e). (Did you know this bug is fixed as of Solr 1.3? That wasn’t my problem today.) You can try the java org.apache.lucene.index.CheckIndex tool with -fix on. Then you should be fine for a while. Until it happens again.
