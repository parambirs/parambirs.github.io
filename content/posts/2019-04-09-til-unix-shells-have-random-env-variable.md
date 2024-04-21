---
title: 'TIL: Unix shells have a $RANDOM env variable'
date: 2019-04-09T00:00:00-07:00
tags: ["unix", "shell"]
summary: "Leverage $RANDOM in Unix shell scripts for controlled randomness and use awk to generate random numbers within defined ranges."
---

Ever wanted to add some randomness to your shell script? No I don't mean to make them flaky! 

Unix shells provide a `$RANDOM` environment variable that we can use in our shell commands or scripts:

```sh
% echo $RANDOM 
21290
% echo $RANDOM
8367
```

The _seed_ for the random number generator can be changed as follows:

```sh
% RANDOM=12345
% echo $RANDOM
5758
```

Generating random numbers in a range
------------------------------------

We can use `awk` for generating random integers in a given range. For example, to generate a random number between 1 and 10 (inclusive):

```sh
% awk -v seed=$RANDOM 'BEGIN{srand(seed); print int(rand()*10+1)}'
10
% awk -v seed=$RANDOM 'BEGIN{srand(seed); print int(rand()*10+1)}'
4
% awk -v seed=$RANDOM 'BEGIN{srand(seed); print int(rand()*10+1)}'
1
```

#### More details at:
- http://srufaculty.sru.edu/david.dailey/unix/random_numbers.htm
