# The Ink Detective Approach to the Vesuvius Challenge

This the documentation of a (for now) abandoned approach to solving the [Vesuvius Challenge](https://scrollprize.org/) in 2024/2025 by [sarahkiener](https://github.com/sarahkiener) and [mpoemsl](https://github.com/mpoemsl).

## The Idea

We decided to tackle the problem of ink detection using already segmented scrolls. The main idea was to try using simple heuristics and knowledge about character bigrams to restrict search space for our algorithm.

## Auxiliary Data Sources

As a source of contemporary bigrams, we decided to use [five epigrams of the Greek philosopher Philodemus](https://www.perseus.tufts.edu/hopper/searchresults?target=greek&all_words=philodemus&all_words_expand=on&phrase=&any_words=&exclude_words=&documents=), since the scrolls were suspected to be by him.

As a source of initialisation for the Sobel filters for Greek letter shapes, we used [a figure of Greek majuscules from a scientific paper](https://www.researchgate.net/figure/Greek-alphabet-of-24-letters_fig1_374386506).

## Preprocessing

Using the epigrams, we derived a bigram matrix with probabilities of each letter pair. The resulting bigram matrix can be found [here](https://ink-detective.github.io/assets/bigram_matrix.csv).

## Algorithm Implementation

## First Results
