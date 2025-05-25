# The Ink Detective Approach to the Vesuvius Challenge

This the documentation of a (for now) abandoned approach to solving the [Vesuvius Challenge](https://scrollprize.org/) in 2024/2025 by [sarahkiener](https://github.com/sarahkiener) and [mpoemsl](https://github.com/mpoemsl).

## The Idea

We decided to tackle the problem of ink detection using already segmented scrolls. The main idea was to try using simple heuristics and knowledge about character bigrams to restrict search space for our algorithm.

## Auxiliary Data Sources

As a source of contemporary bigrams, we decided to use [five epigrams of the Greek philosopher Philodemus](https://www.perseus.tufts.edu/hopper/searchresults?target=greek&all_words=philodemus&all_words_expand=on&phrase=&any_words=&exclude_words=&documents=), since the scrolls were suspected to be by him.

As a source of initialisation for Greek letter shapes, we used [a figure of Greek majuscules from a scientific paper](https://www.researchgate.net/figure/Greek-alphabet-of-24-letters_fig1_374386506).

## Preprocessing

Using the epigrams, we derived a bigram matrix with probabilities of each letter pair. The resulting bigram matrix can be found [here](https://ink-detective.github.io/assets/bigram_matrix.csv).

## Algorithm

To keep it simple and in line with our small computational resources, we decided to start using a [Sobel filter](https://en.wikipedia.org/wiki/Sobel_operator) approach. The initial filter values were derived from the Greek letter shapes mentioned earlier. As a data augmentation step, we layered different rotations (+-5 degrees) and distinguish outside empty spaces and inside empty spaces using a [flood fill](https://en.wikipedia.org/wiki/Flood_fill) approach.

After some experimentation with different convolution methods, we settled on [scipy.signal.fftconvolve](https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.fftconvolve.html), which uses fast Fourier transform to obtain convolution.

## First Results

Applying the algorithm to real scroll data proved to be error-prone, especially when we started looking for bigrams. As a validation step before, we tried the algorithm on both a test file of modern Greek handwritings and a collation of last year's winning approach.

![Modern Greek handwritings](https://ink-detective.github.io/assets/test_image.jpg)

![Last Year's Results](https://ink-detective.github.io/assets/single_letter_findings.png)


## Conclusion
