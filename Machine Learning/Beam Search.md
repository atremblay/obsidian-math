


A widely adopted application of beam search is in encoder-decoder models such as machine translation. When translating one language to another, we first encode a sequence of words from the source language and then decode the intermediate representation into a sequence of words in the target language. In the decoding process, for each word in the sequence, there can be several options. This is where the beam search comes into play.

Let’s go through a simplified sample decoding process step by step. In this example, our heuristic function will be the probability of words and phrases that are possible in translation. First, we start with a token that signals the beginning of a sequence. Then, we can obtain the first word by calculating the probability of all the words in our vocabulary set. We choose $\beta=2$ (beam width) words with highest probabilities as our initial words (`{arrived, the}`):

> [!blank-container|right-large]
> 
> ![beamsearch](https://www.baeldung.com/wp-content/uploads/sites/4/2021/10/beamsearch.jpg)
> 

After that, we expand the two words and compute the probability of other words that can come after them. The words with the highest probabilities will be (`{arrived the, arrived witch, the green, the witch}`). From these possible paths, we choose the two most probable ones (`{the green, the witch}`). Now we expand these two and get other possible combinations (`{the green witch, the green mage, the witch arrived, the witch who}`). Once again, we select two words that maximize the probability of the current sequence (`{the green witch, the witch who}`). We continue doing this until we reach the end of the sequence. In the end, we’re likely to get the most probable translation sequence.

We should keep in mind that along the way, by choosing a small $\beta$, we’re ignoring some paths that might have been more likely due to long-term dependencies in natural languages. One way to deal with this problem is to opt for a larger beam width.

https://www.baeldung.com/cs/beam-search
https://web.stanford.edu/~jurafsky/slp3/ed3book.pdf

