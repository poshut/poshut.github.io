---
layout: post
title: "Why hashing hashes is bad"
date: 2018-07-23 22:42:00 +0200
categories: cryptography, security
---

## What is a hash function?

Even in the days of ```bcrypt``` and other variable-difficulty hashing algorithms, many people still apply hash functions like ```SHA256``` over and over again to "increase its security". Not only is this a waste of time, but it also decreases the security of their hash functions.

A hash function is a function that takes variable-length input and produces fixed-length input. A good hash function should

* should produce the same output given the same input
* change its value in a seemingly random fashion when changing the input even slightly
* have few **collision**s.


## Collisions

A collision occurs when the hash function produces the same output given different inputs; collisions should occur as rarely as possible to provide good security against what are called **collision attacks**, where an attacker tries to manipulate a system relying on hashes, for example a download site where the ```SHA256``` of the files is used to check for errors. If the attacker found a hash collision of the official file, and an own file containing malware, he could replace the official file with his own without anyone noticing since the hash is still the same.

By definition, every hash function has more possible inputs than outputs. Therefore, collisions are unavoidable and the measure for how secure it is is the time it takes to find such a collision.

When we are hashing information to assure users of its correctness, we want to make it as hard as possible for an attacker to find a hash collision. One way we could do this is by using a longer hash function of the same family which requires the attacker to calculate more hashes before finding a collision. Another way would be to use a hash function that takes longer to compute, therefore slowing down the attacker.


## Why not reapply a hash function?

One way people have been implementing the later method is to apply the hash function on its own output over and over again. The fallacy with this is that they only look at the time, not at the amount of possible hashes after each hash step.

Let's consider hash functions to be perfectly random. Therefore, given an input value, out imaginary hash function has an equal probability of having any of its possible outputs.

The problem with re-applying a hash function onto its output is that it is almost impossible that there won't be collisions between the outputs it is applied on.

For this, let's consider the [**Birthday Problem**](https://en.wikipedia.org/wiki/Birthday_problem). The birthday problem asks how many people need to be a room so that there is a probability of 50% that two people will share the same birthday, assuming birthdays are randomly distributed amongst the year. The answer comes pretty surprising for most people: 23.

We can apply the birthday problem to hashing and ask: How many possible output values of a hashing function do we have to re-hash so that there is a probability of 50% to find a hash collision? The function that relates an amount of hashes to the probability of finding a collision rises very fast and reaches 0.999 with a proportion of 0.2 possible output values hashed. It is needless to say that we will be experiencing a large amount of collisions when we hash every possible output value.

Now, we have a decreased pool of possible hashes. Since the hash function is not allied once onto its output but multiple times, we the same thing happens, but differently. Starting out in the first step, we had every hash the hash function allows as inputs. Since our hash pool decreased, we now have fewer distinct input values. But since the hash function already has a high probability of having collisions at 0.2 of its own output set as inputs, we will again have hash collisions of fewer possible hashes. This means that the hash pool decreases again.

Because of this, every time we hash the output value of the same hash function, we decrease the amount of hashes we could have at this point. This works against the "advantage" that an attacker would have to hash an input multiple times.



<img src="{{'/assets/non_injective_or_surjective.png' | absolute_url }}" alt="Hashing a hash is neither surjective nor injective" width="300" />

Hashing a hash looks like this. It does not preserve distinctness.

## A solution

For years, there have been solutions like ```bcrypt``` to offer hashing with a variable amount of iterations to compute the hash. ```bcrypt``` does not decrease its pool of possible output values on each iteration and is an almost-standard way to hash passwords.

