# The Ink Detective Approach to the Vesuvius Challenge

This is the documentation of a (for now) abandoned approach to solving the [Vesuvius Challenge](https://scrollprize.org/) in 2024/2025 by [sarahkiener](https://github.com/sarahkiener) and [mpoemsl](https://github.com/mpoemsl).

## The Idea

We decided to tackle the problem of ink detection using already segmented scrolls. The main idea was to try using simple heuristics and knowledge about character bigrams to restrict search space for our algorithm.

## Auxiliary Data Sources

As a source of contemporary bigrams, we decided to use [five epigrams of the Greek philosopher Philodemus](https://www.perseus.tufts.edu/hopper/searchresults?target=greek&all_words=philodemus&all_words_expand=on&phrase=&any_words=&exclude_words=&documents=), since the scrolls were suspected to be by him.

As a source of initialization for Greek letter shapes, we used [a figure of Greek majuscules from a scientific paper](https://www.researchgate.net/figure/Greek-alphabet-of-24-letters_fig1_374386506).

## Preprocessing

Using the epigrams, we derived a bigram matrix with probabilities of each letter pair. The resulting bigram matrix can be found [here](https://ink-detective.github.io/assets/bigram_matrix.csv). The most common bigrams according to our findings were:

* Α Ι
* Ο Ν
* Ω Ν
* Κ Α
* Τ Ο

## Algorithm

To keep it simple and in line with our small computational resources, we decided to use a [Sobel filter](https://en.wikipedia.org/wiki/Sobel_operator) approach. The initial filter values were derived from the Greek letter shapes mentioned earlier. As a data augmentation step, we layered different rotations (+-5 degrees) and distinguished outside empty spaces from inside empty spaces using a [flood fill](https://en.wikipedia.org/wiki/Flood_fill) approach.

After some experimentation with different convolution methods, we settled on [scipy.signal.fftconvolve](https://docs.scipy.org/doc/scipy/reference/generated/scipy.signal.fftconvolve.html), which uses fast Fourier transform to obtain convolution.

## First Results

Applying the algorithm to real scroll data proved to be error-prone, especially when we started looking for bigrams. As a validation step before, we tried the algorithm on both a test file of modern Greek handwritings and a collation of last year's winning approach.

![Modern Greek handwritings](https://ink-detective.github.io/assets/test_image.jpg)

![Last Year's Results](https://ink-detective.github.io/assets/single_letter_findings.png)

Some issues with these results:

* Size of boxes is unclear - brute-forceing multiple sizes adds computational complexity
* Overlapping boxes - unclear which letter has the strongest claim on a given box
* Wrong positives on areas of high white value accumulation

As a next step, we identified adjacent boxes via a definition of neighborhood. These were considered as potential text or title beginnings. We considered boxes to be neighbors if:

* They came from the same layer
* They had the same letter scale
* They were at least one box width and at most 1.5 box widths apart
* They were at most 0.5 box heights apart

We also factored in the a priori bigram probabilities from the bigram matrix to bias the results towards likely bigrams.

![Successful Bigram Detection](https://ink-detective.github.io/assets/successful_bigram.png)
Depicted above, one successful bigram detection featuring Υ and Η.

However, we didn't really succeed in finding bigrams in the actual scroll data.

Some reasons for our failure to find bigrams in the actual scroll data:

* The selected Sobel filter algorithm was too coarse and not very successful at identifying vague shapes as letters
* The scale of the scroll data is massive. We moved to Azure, but costs for data hosting alone turned out to be larger than expected. We did not have access to GPUs. At some point we even ran into RAM issues.

## Conclusion

It was interesting to try to participate in such a challenge with comparatively simple methods. It turned out the project was a bit too complex to be solved with the given computational resources and time availabilities. However, it was fun and we learned a lot.
