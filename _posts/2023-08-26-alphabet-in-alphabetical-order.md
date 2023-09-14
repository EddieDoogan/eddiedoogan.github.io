---
layout: post
title:  "Investigating Invariant Alphabets"
date:   2023-08-22 21:15:50 +0100
categories: maths
published: false
---

## Putting the Alphabet in Alphabetical Order / The Idea
I was on my phone and saw a post featuring a [viral tweet][neil-tweet] by Neil deGrasse Tyson, stating that the alphabet in alphabetical order is as follows:
> A H R B Q D W E F L M N S X G I J K O P C T V Y U Z

So, what did he mean by this? 
I think he was refering to giving each letter in the alphabet a spelling corresponding to how you pronounce it and then placing those spellings in alphabetical order. 
For exaple the letter 'h' could be spelled 'aitch' and so when the spellings are put in alphabetical order, 'h' goes at the start of the alphabet (where 'a' was).
I couldn't find a source for the spellings that where used in the tweet so I instead decided to pick my own as are listed below:

| Letter | Spelling | Reduced Spellings |
| :----: | :------: | :---------------: |
| a | ay | ay |
| b | bee | b |
| c | see | s |
| d | dee | de |
| e | ee | ee |
| f | eff | ef |
| g | jee | je |
| h | aitch | ai |
| i | eye | ey |
| j | jay | ja |
| k | kay | k |
| l | ell | el |
| m | em | em |
| n | en | en |
| o | oh | o |
| p | pee | p |
| q | cue | c |
| r | are | ar |
| s | ess | es |
| t | tee | te |
| u | you | y |
| v | vee | v |
| w | doubleyou[^w] | do |
| x | ex | ex |
| y | why | w |
| z | zed | z |

# Alphabetical Order
Putting this into alphabetical order yields an alphabet as follows:
> H R A B Q D W E F L M N S X I J G K O P C T V Y U Z

## Repeated Alphabetising
Now that the alphabet has been put into a new order, we have a new alphabet. Let's call the original alphabet (ABCD...) the 0-bet and the new alphabet (HRAB...) the 1-bet. The 1-bet is in alphabetical order according to the 0-bet but since it does not have the same order as the 0-bet it is not in alphabetical order according to itself. I want to find an alphabet that is in alphabetical order according to itself and then there would be a [known reason][order-reason] behind the order. I'm going to call an alphabet that is in order according to itself an 'invariant alphabet'.

# Alphabetise
We can think of a function 'alphabetise' that, when given an alphabet will put it into alphabetical order according to itself. Then the fixed points of this function will be invariant alphabets.

# Finding an invariant alphabet
There are many approaches we can go about trying to find an invariant alphabet the first and simplest one that came to mind for me was iteration. Start with an initial alphabet and alphabetise it to obtain a new alphabet then alphabetise that new alphabet to create another alphabet. Keep alphabetising the new alphabets until the alphabet created is the same as the one before it. An algorithm for this is written below:

    alphabet = initialAlphabet

    while(alphabet isn't invariant)
        alphabet = Alphabetise(alphabet)
    
    print(alphabet)

# Iteration
The above pseudocode would be able to find an invariant alphabet if the iterations happen to land on one but that won't always be the case. To help illustrate why, let's look at an example. Suppose we have an alphabet with only 3 letters spelled as follows:

| Letter | Spelling |
| :----: | :------: |
| f | heff |
| g | gee |
| h | faitch |

We can simulate the first 4 iterations of the invariant alphabet finder algorithm to yield the first 4 created alphabets:

| Alphabet 1 | Alphabet 2 | Alphabet 3 | Alphabet 4 |
| :--------: | :--------: | :--------: | :--------: |
| h | f | h | f |
| g | g | g | g |
| f | h | f | h |

If we did run the invariant alphabet finder on this, it would never end, instead it would continue swapping between alphabet 1 and alphabet 2.

[comment]: <We've discovered that our current function can fail if it reaches a loop of alphabets which each flow into eachother but are there any other circumstances in which it could fail? If we just think of an alphabet as a specific number of letters in a specific order, we can see that there is a finite number of possible number of orders with a given set of letters. Because each alphabet is alphabetised>

# New Pseudocode
In order to eliminate the possibility of our algorithm getting stuck in a loop, we need to check if we have ever seen one of the alphabets before and if we have we can note that we have failed to find an invariant alphabet. The pseudocode for this new algorithm is below:

    alphabet = initialAlphabet

    while(alphabet isn't in listOfAlphabets)
        add alphabet to listOfAlphabets
        alphabet = Alphabetise(alphabet)
    
    if(alphabet = alphabetise(alphabet)): print(alphabet)
    else: print("The alphabet turned ended up in a loop")

## Running The Code

# Hypothesis
My initial hypothesis was that I would fail to find an invariant alphabet immediately and that I would instead have to either alter the order of the starting alphabet in order to find one, or to approach the problem more analytically.

# Results
When the program is run it outputs the following invariant alphabet:
> R A H B D W Y U I E F L M N S X C Q J G K O P T V Z

# Altering initial conditions
I was surprised to find an invariant alphabet immediately and so I decided to try changing the order of the letters in the initial alphabet to see what would happen, and again I found an invariant alphabet, although not the same one this time. By repeatedly running the algorithm with different starting conditions I repeatedly found invariant alphabets but never any loops of alphabets. Infact after running the code 100,000 times with random starting conditions not once did I find a loop. The different invariant alphabets I did find seemed quite varied, the first five that I found are listed below:

| Alphabet |
| -------- |
| R A H V D W Y U K P I M L E F S N X C Q B T O Z J G |
| E F M X S N L I C Q V Z H R A D W Y U G J B O K P T |
| E N X F L M S I C Q P T H R A B D W Y U Z G J O V K |
| T O R A H V U Y W D Q C I L S F E M N X J G P B K Z |
| V R H A P E N X F L M S I C Q Z D W Y U T B K J G O |

## Analysis 
# Why All Invariant?
The next big question I had was if all the alphabets with this spelling led to an invariant one and if so, why. I thought about searching for an alphabet loop from every possible initial alphabet but since there are over 400 quadrillion combinations (26!) it isn't feasible, at least not without greatly improving the efficiency of my program. 

# Groups With Starting Letters
The key to unravelling why all the alphabets lead to invariance is to look at the invariant alphabets listed above. There are some groups of letters that always clump together. For example the letters 'h', 'r' and 'a' always appear next to each other but not necessarily in the same order. This makes sense as all 3 of these words have the same starting letter of 'a'. By separating words based on their starting letters we get the following groups:

| A | B | C | D | E | J | K | O | P | S | T | V | W | Y | Z |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| H | B | Q | D | E | J | K | O | P | C | T | V | Y | U | Z |
| R |   |   | W | F | G |   |   |   |   |   |   |   |   |   |
| A |   |   |   | L |   |   |   |   |   |   |   |   |   |   |
|   |   |   |   | M |   |   |   |   |   |   |   |   |   |   |
|   |   |   |   | N |   |   |   |   |   |   |   |   |   |   |
|   |   |   |   | S |   |   |   |   |   |   |   |   |   |   |
|   |   |   |   | X |   |   |   |   |   |   |   |   |   |   |
|   |   |   |   | I |   |   |   |   |   |   |   |   |   |   |

# How do letters move
It is useful to consider how a given letter will 'move' when the alphabetise method is used take the same example from earlier where we have an alphabet with letters and spellings as follows:

| Letter | Spelling |
| :----: | :------: |
| f | heff |
| g | gee |
| h | faitch |

When this alphabet is alphabetised with any starting order the following will happen:
- 'f' will move to where 'h' is.
- 'g' will move to where 'g' is (in some sense staying put).
- 'h' will move to where 'f' is.

This diagram shows each letter with an arrow pointing where it will move to.

![Letters in bubbles pointing where they are going](/assets/images/SampleAlphabet.png)

The diagram shows two loops formed, one containing 'f' and 'h' and the other containing just 'g'. If a diagram contains a loop of two or more letters it will not be possilbe to find an invariant alphabet, instead those letters will keep 'chasing' eachother and will move everytime alphabetise is used.


[neil-tweet]:https://twitter.com/neiltyson/status/259486092766625793?lang=en
[^w]: 'w' is spelled 'doubleyou' without a space as spaces aren't in the alphabet. Although, as seen from the reduced spellings, it could still be ordered even if the non-alphabetic characters were used.
[order-reason]:https://www.southernliving.com/news/alphabetical-order-history
[fixed-point-iteration]:https://en.wikipedia.org/wiki/Fixed-point_iteration