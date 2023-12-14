# StemGen: A music generation model that listens
Julian Parker, Janne Spijkervet, Katerina Kosta, Furkan Yesiler, Boris Kuznetsov, Ju-Chiang Wang, Matt Avent, Jitong Chen, Duc Le

Accepted at ICASSP24

## Contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

## Overview
StemGen is an end-to-end music generation model, trained to listen to musical context and respond appropriately. It's built on a non-autoregressive language-model type architecture similar to [SoundStorm](https://google-research.github.io/seanet/soundstorm/examples/) and [VampNet](https://hugo-does-things.notion.site/VampNet-Music-Generation-via-Masked-Acoustic-Token-Modeling-e37aabd0d5f1493aa42c5711d0764b33) . More details in the paper: arxiv link

## Models / datasets

We present examples from three different models here:

|Name|Dataset|Conditioning|Tokenizer|
|-|-|-|-|
|`slakh` | [Slakh2100](http://www.slakh.com) | Target instrument category| [32kHz Encodec](https://github.com/facebookresearch/audiocraft)
|`internal`| internal dataset of 500 hours of human-played music available in individual instrument stems| Target instrument category| [32kHz Encodec](https://github.com/facebookresearch/audiocraft)
|`mingus` *|  Pretrained on 500 hours of synthetic data from an internal symbolic music generation model. Fine tuned on 2hrs of high quality human composed and produced music.| Genre category, target stem category| [stereo 48kHz Encodec](https://github.com/facebookresearch/audiocraft)

\* not presented in paper
## Test set examples

These examples are produced by constructing context audio from the test dataset partition, and using a model to generate a single stem (conditioning chosen at random) in response. They therefore closely reflect the task presented to the model at training time.

|Model|`slakh`| `slakh`| 
|-|-|-|
|Target category|Guitar| Guitar |
|Context|  <audio controls preload="metadata"><source src="../examples/sounds/test_set/slakh/163-source.mp3" type="audio/mpeg">Your browser does not support the audio element.</audio>| <audio controls preload="metadata"><source src="../examples/sounds/test_set/slakh/194-source.mp3" type="audio/mpeg">Your browser does not support the audio element.</audio>|
|Generated stem| <audio controls preload="metadata"><source src="../examples/sounds/test_set/slakh/163-sampled-stem-3.mp3" type="audio/mpeg">Your browser does not support the audio element.</audio> |  <audio controls preload="metadata"><source src="../examples/sounds/test_set/slakh/194-sampled-stem-3.mp3" type="audio/mpeg">Your browser does not support the audio element.</audio> |
|Mixed| <audio controls preload="metadata"><source src="../examples/sounds/test_set/slakh/163-mixed.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> | <audio controls preload="metadata"><source src="../examples/sounds/test_set/slakh/194-mixed.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |

|Model|`slakh`| `slakh`| 
|-|-|-|
|Target category|Synth| Drums |
|Context|  <audio controls preload="metadata"><source src="../examples/sounds/test_set/slakh/22-source.mp3" type="audio/mpeg">Your browser does not support the audio element.</audio>| <audio controls preload="metadata"><source src="../examples/sounds/test_set/slakh/23-source.mp3" type="audio/mpeg">Your browser does not support the audio element.</audio>|
|Generated stem| <audio controls preload="metadata"><source src="../examples/sounds/test_set/slakh/22-sampled-stem-10.mp3" type="audio/mpeg">Your browser does not support the audio element.</audio> |  <audio controls preload="metadata"><source src="../examples/sounds/test_set/slakh/23-sampled-stem-1.mp3" type="audio/mpeg">Your browser does not support the audio element.</audio> |
|Mixed| <audio controls preload="metadata"><source src="../examples/sounds/test_set/slakh/22-mixed.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> | <audio controls preload="metadata"><source src="../examples/sounds/test_set/slakh/23-mixed.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |

|Model|`slakh`| `slakh`| 
|-|-|-|
|Target category|Drums| Drums |
|Context|  <audio controls preload="metadata"><source src="../examples/sounds/test_set/slakh/26-source.mp3" type="audio/mpeg">Your browser does not support the audio element.</audio>| <audio controls preload="metadata"><source src="../examples/sounds/test_set/slakh/111-source.mp3" type="audio/mpeg">Your browser does not support the audio element.</audio>|
|Generated stem| <audio controls preload="metadata"><source src="../examples/sounds/test_set/slakh/26-sampled-stem-1.mp3" type="audio/mpeg">Your browser does not support the audio element.</audio> |  <audio controls preload="metadata"><source src="../examples/sounds/test_set/slakh/111-sampled-stem-1.mp3" type="audio/mpeg">Your browser does not support the audio element.</audio> |
|Mixed| <audio controls preload="metadata"><source src="../examples/sounds/test_set/slakh/26-mixed.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> | <audio controls preload="metadata"><source src="../examples/sounds/test_set/slakh/111-mixed.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |

|Model|`internal`| `internal`| 
|-|-|-|
|Target category|Guitar| Drums |
|Context|  <audio controls preload="metadata"><source src="../examples/sounds/test_set/internal/234-source.mp3" type="audio/mpeg">Your browser does not support the audio element.</audio>| <audio controls preload="metadata"><source src="../examples/sounds/test_set/internal/392-source.mp3" type="audio/mpeg">Your browser does not support the audio element.</audio>|
|Generated stem| <audio controls preload="metadata"><source src="../examples/sounds/test_set/internal/234-sampled-stem-24.mp3" type="audio/mpeg">Your browser does not support the audio element.</audio> |  <audio controls preload="metadata"><source src="../examples/sounds/test_set/internal/392-sampled-stem-128.mp3" type="audio/mpeg">Your browser does not support the audio element.</audio> |
|Mixed| <audio controls preload="metadata"><source src="../examples/sounds/test_set/internal/234-mixed.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> | <audio controls preload="metadata"><source src="../examples/sounds/test_set/internal/392-mixed.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |


## Iterative generation examples

These examples are produced by providing the models with an arbitrary piece of context audio as a starting point. A new stem is generated from that context, and mixed with the existing audio. This new mixed audio is used as the context to generate another stem, and the process repeats. These examples therefore represent a much more challenging situation for the models, as they need to listen to and interpret both out-of-distribution audio and their own output.

These examples reflect a more typical use-case of StemGen, with a user constructing music iteratively in a chat-like environment. In order to preserve how this interactive process proceeded, we also show situations where the user generated multiple variations of a particular stem. These are denoted as variations of the iteration. When one is chosen to proceed for further iteration, it is marked in *italics*

###  Starting from drums
In this example, we use a short drum loop composed by the authors in Ableton Live as a starting point.

 <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/slakh/layering_from_drums/start.wav" type="audio/mpeg">Your browser does not support the audio element.</audio>

This example is chosen to demonstrate how the models follow rhythm, and also how they can generate coherant harmonic and melodic elements even with no harmonic or melodic information provided at the start of the process.

#### `slakh` 

|Iteration|1| 2 (var. 1)| 2 (var. 2)| 2 (var. 3)| 
|-|-|-|-|-|
|Conditioning|Bass| Piano | Piano | Woodwind |
|Context|  <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/slakh/layering_from_drums/start.wav" type="audio/mpeg">Your browser does not support the audio element.</audio>| <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/slakh/layering_from_drums/1-mix.wav" type="audio/mpeg">Your browser does not support the audio element.</audio>|<audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/slakh/layering_from_drums/1-mix.wav" type="audio/mpeg">Your browser does not support the audio element.</audio>|<audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/slakh/layering_from_drums/1-mix.wav" type="audio/mpeg">Your browser does not support the audio element.</audio>|
|Generated stem| <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/slakh/layering_from_drums/1-stem.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |  <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/slakh/layering_from_drums/2-var-1-stem.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |<audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/slakh/layering_from_drums/2-var-2-stem.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |<audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/slakh/layering_from_drums/2-var-3-stem.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
|Mixed| <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/slakh/layering_from_drums/1-mix.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> | <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/slakh/layering_from_drums/2-var-1-mixed.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |<audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/slakh/layering_from_drums/2-var-2-mixed.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |<audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/slakh/layering_from_drums/2-var-3-mixed.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |

#### `internal` 

*Italic denotes which variation was used to continue generation*

|Iteration| 1 (var. 1) | *1 (var. 2)* | 2| 3 | 
|-|-|-|-|-|
|Conditioning|Guitar| Guitar | Drums | Bass |
|Context|  <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/internal/layering_from_drums/start.wav" type="audio/mpeg">Your browser does not support the audio element.</audio>| <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/internal/layering_from_drums/start.wav" type="audio/mpeg">Your browser does not support the audio element.</audio>|<audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/internal/layering_from_drums/1-var-2-mix.wav" type="audio/mpeg">Your browser does not support the audio element.</audio>|<audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/internal/layering_from_drums/2-mix.wav" type="audio/mpeg">Your browser does not support the audio element.</audio>|
|Generated stem| <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/internal/layering_from_drums/1-var-1-stem.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |  <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/internal/layering_from_drums/1-var-2-stem.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |<audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/internal/layering_from_drums/2-stem.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |<audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/internal/layering_from_drums/3-stem.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
|Mixed| <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/internal/layering_from_drums/1-var-1-mix.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> | <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/internal/layering_from_drums/1-var-2-mix.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |<audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/internal/layering_from_drums/2-mix.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |<audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/internal/layering_from_drums/3-mix.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |

#### `mingus` 

|Iteration|1| 2 | 3 | 4 | 
|-|-|-|-|-|
|Conditioning|Electronic, Melodic| Electronic, Harmonic | Electronic, Harmonic | Electronic, Percussive |
|Context|  <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/mingus/layering_from_drums/start.wav" type="audio/mpeg">Your browser does not support the audio element.</audio>| <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/mingus/layering_from_drums/1-mix.wav" type="audio/mpeg">Your browser does not support the audio element.</audio>|<audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/mingus/layering_from_drums/2-mix.wav" type="audio/mpeg">Your browser does not support the audio element.</audio>|<audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/mingus/layering_from_drums/3-mix.wav" type="audio/mpeg">Your browser does not support the audio element.</audio>|
|Generated stem| <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/mingus/layering_from_drums/1-stem.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |  <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/mingus/layering_from_drums/2-stem.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |<audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/mingus/layering_from_drums/3-stem.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |<audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/mingus/layering_from_drums/4-stem.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |
|Mixed| <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/mingus/layering_from_drums/1-mix.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> | <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/mingus/layering_from_drums/2-mix.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |<audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/mingus/layering_from_drums/3-mix.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |<audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/mingus/layering_from_drums/4-mix.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |

###  Starting from chords

In this example, we use a short synth chord sequence composed by the authors in Ableton Live as a starting point.

<audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/slakh/layering_from_chords/start.wav" type="audio/mpeg">Your browser does not support the audio element.</audio>

This example is targeted to show how the models can respond to harmony in a musically plausibly way.

#### `slakh` 

|Iteration|1| 2| 
|-|-|-|
|Conditioning|Piano| Drums |
|Context|  <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/slakh/layering_from_chords/start.wav" type="audio/mpeg">Your browser does not support the audio element.</audio>| <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/slakh/layering_from_chords/1-mixed.wav" type="audio/mpeg">Your browser does not support the audio element.</audio>
|Generated stem| <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/slakh/layering_from_chords/1-stem.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |  <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/slakh/layering_from_chords/2-stem.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> 
|Mixed| <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/slakh/layering_from_chords/1-mixed.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> | <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/slakh/layering_from_chords/2-mixed.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> 

#### `internal` 

|Iteration|1| 2| 3|
|-|-|-| - |
|Conditioning|Guitar| Guitar | Bass|
|Context|  <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/internal/layering_from_chords/start.wav" type="audio/mpeg">Your browser does not support the audio element.</audio>| <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/internal/layering_from_chords/1-mix.wav" type="audio/mpeg">Your browser does not support the audio element.</audio>| <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/internal/layering_from_chords/2-mix.wav" type="audio/mpeg">Your browser does not support the audio element.</audio>
|Generated stem| <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/internal/layering_from_chords/1-stem.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |  <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/internal/layering_from_chords/2-stem.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> |  <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/internal/layering_from_chords/3-stem.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> 
|Mixed| <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/internal/layering_from_chords/1-mix.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> | <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/internal/layering_from_chords/2-mix.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> | <audio controls preload="metadata"><source src="../examples/sounds/iterative_generation/internal/layering_from_chords/3-mix.wav" type="audio/mpeg">Your browser does not support the audio element.</audio> 


###  Melody layering
#### `slakh` 

|Iteration| Conditioning| Context| Generated|
|-|-|-|-|


## Live interactive music generation demos

These demos are of a p


| Lofi | Techno |
|-------|-------|
| <video width="320" height="240" controls><source src="../examples/videos/realtime_lofi.mov" type="video/mp4">Your browser does not support the video tag.</video> | <video width="320" height="240" controls><source src="../examples/videos/realtime_tech.mov" type="video/mp4">Your browser does not support the video tag.</video>|