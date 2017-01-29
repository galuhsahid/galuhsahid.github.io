---
layout: post
title:  "Sargability, or why %LIKE% is evil"
date:   2017-01-04
tag: SQL
blog: true
---

When you're working with big data, performance becomes a really huge deal. Suddenly extracting the information that you need isn't merely querying or writing scripts that, at least, work. They have to both *work* and be as *efficient* as possible.

Previously I wrote that I was working on a script to evaluate a transformed dataset, which involved joining tables and, unfortunately, full text search: 

```python
df_count = spark.sql("SELECT id, COUNT(hit_rules) as `hit_rules_count` FROM rules LEFT OUTER JOIN transactions ON INSTR(hit_rules, id) > 0 GROUP BY id ORDER BY id")
```

The reason why I had to use either `%LIKE%` with wildcards on both ends or `INSTR` was the nature of the dataset itself. A particular attribute contains values like this one:

```
- - SC012222 - SC012242 - SC012322
```

And it just happened that the required information needed to be extracted *really* depends on those SC values, *individually*. Well, the whole full text search shenanigans could have been avoided with a change of the database design but unfortunately that wasn't an option.

## Evaluating the options: LIKE, INSTR, and REGEXP
When faced with problems involving querying with pattern matching, there were a few options that we can use: LIKE, INSTR, and REGEXP. Somehow, no idea why, regex has always been the natural solution to me (probably because I've been using it extensively lately for other purposes) so that was my first try. It was, though, the *slowest* among the three: 

That left me with two other options: LIKE and INSTR, both of which performed exactly the same--at least in *my* case.

But still, my script took forever to load. The fact that the join had to result in cartesian products didn't really help, either. Curiosity killed the cat, and it turned out that in this case, the cartesian product wasn't the only culprit. 

## Meet sargability
After a few quick readings it became painfully obvious that `%LIKE%`--with the wildcard on *both* ends is evil. The reason has something to do with sargability, which turns out to be an actual word. 