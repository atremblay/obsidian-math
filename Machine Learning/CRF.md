[Original paper](https://repository.upenn.edu/cgi/viewcontent.cgi?article=1162&context=cis_papers)

Sequential data can be made conditionnal on other timesteps by using a conditionnal random field (CRF).

A good implementation in Pytorch is available [here](https://github.com/kmkurn/pytorch-crf/tree/e843718540512eb04d9b756fca04a0915affe175)

At every timestep we have the following conditionnal probility where $x^{(t)}$ is the output of the previous layer in a Neural Network. Note that CRF are not exclusive to NN, but we will think about it in this context.

$$p\left(y^{(t)}\big|\textbf{x}^{(t)}\right) = \text{softmax}\left(\textbf{x}^{(t)}\right)$$

$y^{(t)}$ can take $d$ values, hence the softmax.

Instead of modeling every timestep independantly we want to output a probability distribution for the whole sequence

$$p\left(\left.\textbf{y}^{(t)}\right|\textbf{X}^{(t)}\right) = p\left(y^{(1)},\ldots,y^{(T)}\big|\textbf{x}^{(1)},\ldots,\textbf{x}^{(T)}\right)$$

Notation

- Training set is $\left\{ \left(\textbf{X}^{(t)}, \textbf{y}^{(t)}\right) \right\}$
- Inputs $\textbf{X}^{(t)} = \left[\textbf{x}_1^{(1)}, \ldots, \textbf{x}_{K_t}^{(t)}\right]$
- targets $\textbf{y}^{(t)} = \left[y_1^{(1)}, \ldots, y_{K_t}^{(t)} \right]$
- $K_t$ is the length of the $t^{th}$ sequence

# Regular classification
In the context of a single sequence, a regular classification at every timestep would model things independently, so 

$$
\begin{align}
p\left(\textbf{y} \big| \textbf{X}\right)&=\prod_{k=1}^K p\left(y_k|\textbf{x}_k\right) \\
&= \prod_{k}\frac{\exp\left(\left(\textbf{x}_k\right)_{y_k}\right)}{Z(\textbf{x}_k)} \\
&= \frac{\exp\left(\sum_k\left(\textbf{x}_k\right)_{y_k}\right)}{\prod_k Z(\textbf{x}_k)}
\end{align}$$

$\exp\left(\left(\textbf{x}_k\right)_{y_k}\right)$ is a bit confusing. $\textbf{x}_k$ is a vector coming from the neural network with size $d$. $y_k$ is the index that $y$ takes at time $k$, i.e. 1 to $d$. So we are taking to exponential of one of the values in $\textbf{x}_k$.

The product is because every timestep is treated as independent. $p\left(\textbf{y} \big| \textbf{X}\right)$ is the probability of a particular sequence of $\textbf{y}$ and $Z(\textbf{x}_k)$ is the normalization constant.

# Sequence classificaton with linear chain

We add a term in the exponential that links $y_k$ and $y_{k+1}$. So if the transition is likely, then that will boost the exponential.

$$
\begin{align}
p\left(\textbf{y} \big| \textbf{X}\right) &= \frac{\exp\left(\sum_k\left(\textbf{x}_k\right)_{y_k} + \sum_{k=1}^{K-1} V_{y_k, y_{k+1}}\right)}{Z(\textbf{X})} \\
&= \frac{\exp\left(\sum_k\left(\textbf{x}_k\right)_{y_k}\right)\exp\left(\sum_{k=1}^{K-1} V_{y_k, y_{k+1}}\right)}{Z(\textbf{X})}
\end{align}
$$

$\left(\textbf{x}_k\right)_{y_k}$ is how likely $y_k$ is given the input. Note, $\textbf{x}_k$ is not the input, this is the last output from the NN. The input could be the tokens in NLP. So a high positive value is likely, negative values are unlikely.

$V$ is a transition matrix and $V_{y_k, y_{k+1}}$ how likely it is to go from $y_k$ to $y_{k+1}$.

So if $y_k$ is likely (high positive value) and $y_{k+1}$ is likely to occur after $y_k$ (high positive value), then the exponential will have a high value.

## Loss function

The probability of a sequence of tags is given by
$$
p\left(\textbf{y} \big| \textbf{X}\right) = \frac{\exp\left(\sum_k\left(\textbf{x}_k\right)_{y_k} + \sum_{k=1}^{K-1} V_{y_k, y_{k+1}}\right)}{Z(\textbf{X})}
$$

During training what we want to do is to maximize the probability of the true targets. We don't really care about all other combinations of tags, we only want to is to maximize the correct path.

If $\textbf{y}^* = \left[y_1^*, \ldots, y_{K_t}^* \right]$ is the correct sequence of tags (coming from the dataset) then we want to maximize the likelihood

$$
p\left(y_1^*, \ldots, y_{K_t}^* \big| \textbf{X}\right) = \frac{\exp\left(\sum_k\left(\textbf{x}_k\right)_{y_k^*} + \sum_{k=1}^{K-1} V_{y_k^*, y_{k+1}^*}\right)}{Z(\textbf{X})}
$$

Or to maximize the log likelihood

$$
\begin{align}
\log{p\left(y_1^*, \ldots, y_{K_t}^* \big| \textbf{X}\right)} &= \log\left(\frac{\exp\left(\sum_k\left(\textbf{x}_k\right)_{y_k^*} + \sum_{k=1}^{K-1} V_{y_k^*, y_{k+1}^*}\right)}{Z(\textbf{X})}\right) \\
&= \log\left(\exp\left(\sum_k\left(\textbf{x}_k\right)_{y_k^*} + \sum_{k=1}^{K-1} V_{y_k^*, y_{k+1}^*}\right)\right) -\log\left(Z(\textbf{X})\right) \\
&= \left(\sum_k\left(\textbf{x}_k\right)_{y_k^*} + \sum_{k=1}^{K-1} V_{y_k^*, y_{k+1}^*}\right) -\log\left(Z(\textbf{X})\right) \\
\end{align}
$$

Or to minimize the negative log likelihood

$$
\begin{align}
-\log{p\left(y_1^*, \ldots, y_{K_t}^* \big| \textbf{X}\right)} 
&= \log\left(Z\left(\textbf{X}\right)\right) -\left(\sum_k\left(\textbf{x}_k\right)_{y_k^*} + \sum_{k=1}^{K-1} V_{y_k^*, y_{k+1}^*}\right)  \\
\end{align}
$$


### Training

The main difference here compared to what we are used to in training a NN is that we need to provide the targets to the `forward()` method so that it can "select" the correct sequence of targets in the emissions and calculate that probability. The method will return the actual probability (or negative log prob) which we directly use as the loss. Instead of outputing something and comparing with the targets and then calculating, for example, the cross-entropy, we instead select the correct stuff from the get go.

In the following example, `tensor(-12.7431, grad_fn=<SumBackward0>)` is the log likelihood. Taking the negative of it and doing a regular backpropagation is what we need.

```python
import torch
from torchcrf import CRF
num_tags = 5  # number of tags is 5
model = CRF(num_tags)

seq_length = 3  # maximum sequence length in a batch
batch_size = 2  # number of samples in the batch
emissions = torch.randn(seq_length, batch_size, num_tags) # would normally come from another layer
tags = torch.tensor([
  [0, 1], [2, 4], [3, 1]
], dtype=torch.long)  # (seq_length, batch_size)
model(emissions, tags) # outputs the log likelihood
# tensor(-12.7431, grad_fn=<SumBackward0>)
```

### Inference

This means that we can't use the `forward()` method to do inference. Typically the emissions are calculated and, in combination with the transition matrix, a Viterbi algorithm is used to output the most likely sequence. The `decode()` method is there just for the inference.

```python
model.decode(emissions)
# [[3, 1, 3], [0, 1, 0]]
```

## Computing $p\left(\textbf{y} \big| \textbf{X}\right)$

### Numerator
There is no way around a good old fashion for loop in python to calculate the inside of the exponential.

```python
# emissions: (seq_length, num_tags) matrix from the NN
# tags: (seq_length,) has the index at each position
# transitions: (num_tags, num_tags)
# start_transitions: (num_tags,)
# end_transitions: (num_tags,)

# Start transition score and first emission
# score will accumulate the value to be exponentiated
score = start_transitions[tags[0]]
score += emissions[0, tags[0]]

for i in range(1, seq_length):
	# Transition score to next tag, only added if next timestep is valid (mask == 1)
	# shape: (batch_size,)
	score += transitions[tags[i - 1], tags[i]]
	
	# Emission score for next tag, only added if next timestep is valid (mask == 1)
	# shape: (batch_size,)
	score += emissions[i, tags[i]]
	
# End transition score
# TODO unclear why this is here.
score += end_transitions[tags[-1]]
```

### Denominator

##### Forward

$Z(\textbf{X})$ is different than the independant case because of the transition factor. It's still the sum over all possible values the numerator can take, but that includes the transition matrix.

We will try to compute it. It's hard. We will switch the notation a bit so it's easier to read

- $a(y_k)=\left(\textbf{x}_k\right)_{y_k}$
- $a(y_k, y_{k+1}) = V_{y_k, y_{k+1}}$

$$
\begin{align}
Z(\textbf{X})&=\sum_{y_1'}\sum_{y_2'}\dots \sum_{y_K'}\exp\left\{\sum_k\left(\textbf{x}_k\right)_{y_k'} + \sum_{k=1}^{K-1} V_{y_k', y_{k+1}'}\right\} \\
&=\sum_{y_1'}\sum_{y_2'}\dots \sum_{y_K'}\exp\left\{\sum_k a(y_k') + \sum_{k=1}^{K-1} a(y_k', y_{k+1}')\right\} \\
&=\sum_{y_1'}\sum_{y_2'}\dots \sum_{y_K'}\exp\left\{\sum_k a(y_k') + \sum_{k=1}^{K-1} a(y_k', y_{k+1}')\right\} \\
&=\sum_{y_1'}\sum_{y_2'}\dots \sum_{y_K'}\left\{\exp\left(\sum_k a(y_k')\right)\exp\left(\sum_{k=1}^{K-1} a(y_k', y_{k+1}')\right)\right\} \\
&=\sum_{y_1'}\sum_{y_2'}\dots \sum_{y_K'}\left\{\left(\prod_k \exp a(y_k')\right)\left(\prod_{k=1}^{K-1}\exp a(y_k', y_{k+1}')\right)\right\}
\end{align} 
$$

The sums can be rearranged and the terms inside can be factored out because they are not part of the sum. Yes, each line is different than the previous one. It's all about rearranging the terms.

$$
\begin{align}
Z(\textbf{X})&=\sum_{y_K'}\sum_{y_{K-1}'}\dots \sum_{y_1'}\left(\left(\prod_k \exp a(y_k')\right)\left(\prod_{k=1}^{K-1}\exp a(y_k', y_{k+1}')\right)\right) \\

&=\sum_{y_K'}\sum_{y_{K-1}'}\dots \sum_{y_1'}\left(\exp a(y_K') \left(\prod_{k=1}^{K-1}\exp a(y_k') \exp a(y_k', y_{k+1}')\right)\right) \\

&=\sum_{y_K'}\sum_{y_{K-1}'}\dots \sum_{y_1'}\left(\exp a(y_K') \left(\prod_{k=1}^{K-1}\exp \left\{a(y_k') + a(y_k', y_{k+1}')\right\}\right)\right) \\

&=\sum_{y_K'}\exp a(y_K')\sum_{y_{K-1}'}\dots \sum_{y_1'}\left(\prod_{k=1}^{K-1}\exp \left\{a(y_k') + a(y_k', y_{k+1}')\right\}\right) \\

&=\sum_{y_K'}\exp a(y_K')
	\left(\sum_{y_{K-1}'}\exp \left\{a(y_{k-1}') + a(y_{k-1}', y_{k}')\right\}
		\left(\sum_{y_{K-2}'}\dots \sum_{y_1'} 
			\left(\prod_{k=1}^{K-2}\exp \left\{a(y_k') + a(y_k', y_{k+1}')\right\}
			\right)
		\right)
	\right) \\

&=\sum_{y_K'}\exp a(y_K') \\
& \qquad \left(  \sum_{y_{K-1}'}\exp \left\{a(y_{k-1}') + a(y_{k-1}', y_{k}')\right\} \right.\\
& \qquad \qquad \dots \\
& \quad \qquad \left( \sum_{y_{2}'}\exp \left\{a(y_{2}') + a(y_{2}', y_{3}')\right\} \right. \\
& \qquad \qquad \left(\left.\left.\left.\sum_{y_1'} exp \left\{a(y_1') + a(y_1', y_{2}')\right\} \right)\right.\right)\dots\right)
\end{align} 
$$

We can start computing the inner sum and store it in a matrix by fixing $y_2'$:

$$\alpha[0, y_2'] = \sum_{y_1'} exp \left\{a(y_1') + a(y_1', y_{2}')\right\}$$ 

$y_k'$ can take 1 of $d$ values, so we simply set $y_2'$ to each one of them, compute the sum and store it. The first position in that vector is the result of the sum over $y_1'$ when we fix $y_2'=1$, second position is the result of the sum over $y_1'$ when we fix $y_2'=2$, etc. 

Then when we calculate the next sum

$$
\begin{align}
\alpha[1, y_3'] &= \sum_{y_2'} exp \left(a(y_2') + a(y_2', y_3')\right) \sum_{y_1'} exp \left\{a(y_1') + a(y_1', y_{2}')\right\} \\
&= \sum_{y_2'} exp \left(a(y_2') + a(y_2', y_3')\right) \alpha[0, y_2']
\end{align}
$$ 

This can be made more efficient by using broadcasting. See the implementation in link above.

```python
import numpy

# emissions: (seq_length, num_tags) matrix from the NN
# tags: (seq_length,) has the index at each position
# transitions: (num_tags, num_tags)

alpha = np.zeros((seq_length, num_tags))

# First inner sum. Start filling the alpha table
for next_tag in range(num_tags):
	for current_tag in range(num_tags):
		alpha[0, current_tag] += np.exp(emissions[i-1, current_tag] + transitions[current_tag, next_tag])

# All other sums except the last one. Built on the previous iteration.
for i in range(1, seq_length - 1):
	for next_tag in range(num_tags):
		for current_tag in range(num_tags):
			alpha[i, current_tag] += (np.exp(emissions[i, current_tag] + transitions[current_tag, next_tag])) * alpha[i-1, current_tag]

# Last sum
for current_tag in range(num_tags):
	alpha[seq_length - 1, current_tag] = np.exp(emissions[seq_length - 1, current_tag] * alpha[seq_length, current_tag]
```

##### Backward

The denominator could also be written backward. Notice the difference in the indiced in the next two equations for the transition matrix. $\sum_{k=1}^{K-1} V_{y_k^*, y_{k+1}^*}$ versus $\sum_{k=2}^{K} V_{y_{k-1}^*, y_{k}^*}$. 

$$
\begin{align}
p\left(y_1^*, \ldots, y_{K_t}^* \big| \textbf{X}\right) &= \frac{\exp\left(\sum_k\left(\textbf{x}_k\right)_{y_k^*} + \sum_{k=1}^{K-1} V_{y_k^*, y_{k+1}^*}\right)}{Z(\textbf{X})} \\
&= \frac{\exp\left(\sum_k\left(\textbf{x}_k\right)_{y_k^*} + \sum_{k=2}^{K} V_{y_{k-1}^*, y_{k}^*}\right)}{Z(\textbf{X})}
\end{align}
$$

Same thing in the end, but that will allow us to arrange the sums in a different order. That will yield a beta table where the summations are starting from the end (i.e. the inner loop starts at $y_K'$ instead of $y_1'$)


$$
\begin{align}
Z(\textbf{X})&=\sum_{y_1'}\sum_{y_2'}\dots \sum_{y_K'}\left(\left(\prod_k \exp a(y_k')\right)\left(\prod_{k=2}^{K}\exp a(y_{k-1}', y_{k}')\right)\right)
\end{align} 
$$

The sums can be rearranged and the terms inside can be factored out because they are not part of the sum. Yes, each line is different than the previous one. It's all about rearranging the terms.

$$
\begin{align}
Z(\textbf{X})&=\sum_{y_1'}\sum_{y_2'}\dots \sum_{y_K'}\left(\left(\prod_k \exp a(y_k')\right)\left(\prod_{k=2}^{K}\exp a(y_{k-1}', y_{k}')\right)\right) \\

&=\sum_{y_1'}\sum_{y_2'}\dots \sum_{y_K'}\left(\exp a(y_1') \left(\prod_{k=2}^{K}\exp a(y_k') \exp a(y_{k-1}', y_{k}')\right)\right) \\

&=\sum_{y_1'}\sum_{y_2'}\dots \sum_{y_K'}\left(\exp a(y_1') \left(\prod_{k=2}^{K}\exp \left\{a(y_k') + a(y_{k-1}', y_{k}')')\right\}\right)\right) \\

&=\sum_{y_1'}\exp a(y_1')\sum_{y_{2}'}\dots \sum_{y_K'}\left(\prod_{k=2}^{K}\exp \left\{a(y_k') + a(y_{k-1}', y_{k}')')\right\}\right) \\

&=\sum_{y_1'}\exp a(y_1')
	\left(\sum_{y_{2}'}\exp \left\{a(y_{1}') + a(y_{1}', y_{2}')\right\}
		\left(\sum_{y_{3}'}\dots \sum_{y_K'} 
			\left(\prod_{k=3}^{K}\exp \left\{a(y_k') + a(y_{k-1}', y_{k}')\right\}
			\right)
		\right)
	\right) \\

&=\sum_{y_1'}\exp a(y_1') \\
& \qquad \left(  \sum_{y_{2}'}\exp \left\{a(y_{1}') + a(y_{1}', y_{2}')\right\} \right.\\
& \qquad \qquad \dots \\
& \quad \qquad \left( \sum_{y_{K-1}'}\exp \left\{a(y_{K-2}') + a(y_{K-2}', y_{K-1}')\right\} \right. \\
& \qquad \qquad \left(\left.\left.\left.\sum_{y_K'} exp \left\{a(y_{K-1}') + a(y_{K-1}', y_{K}')\right\} \right)\right.\right)\dots\right)
\end{align} 
$$

#### log-sum-exp

Because summing exponential like this is prone to numerical instability, we can use the log-sum-exp trick. Rewritting the specific example from above

$$
\begin{align}
\alpha[1, y_3'] &= \sum_{y_2'} exp \left(a(y_2') + a(y_2', y_3')\right) \alpha[0, y_2'] \\
&= \sum_{y_2'} exp \left(a(y_2') + a(y_2', y_3')\right) \exp\left(\log \alpha[0, y_2']\right) \\
&= \sum_{y_2'} exp \left(a(y_2') + a(y_2', y_3') + \log \alpha[0, y_2']\right) \\
\end{align}
$$ 

Change of variable $z_{y_2'} = a(y_2') + a(y_2', y_3') + \log \alpha[0, y_2']$
$$
\begin{align}
\log \alpha[1, y_3'] &= \log \sum_{y_2'} exp \left(z_{y_2'}\right) \\
&= \underbrace{\max_{y_2'}\left(z_{y_2'}\right) + \log \sum_{y_2'} exp \left(z_{y_2'} - \max_{y_2'}\left(z_{y_2'}\right)\right)}_{\text{numerically stable}}
\end{align}
$$ 

What does this mean for the implementation? Better look at `pytorch-crf` in `_compute_normalizer()`. The gist of it is to store the values in a matrix and then call `torch.logsumexp` on it. In the code snippet above we do an inplace sum on the exponential. Instead, store that in a matrix and do the log-sum-exp trick.

# Context window

The $\textbf{x}_k$ are generated from the NN at each step. They could be sequences of images where each images are using the same NN or they could be output from an RNN or BERT.

In the case of sequence of images, we could link the outputs $\textbf{x}_{k-1}, \textbf{x}_{k}, \textbf{x}_{k+1}$ in another layer to get a link between the previous step and the next one. Kind of a convolution. Or a fully connected or anything that will output a vector of the right size.

In the case of RNN, same thing can be done. Be design a RNN only looks at the past. If we use a bi-directionnal RNN then we see both the past and present.

BERT uses the transformer and the transformer attend to every positions in the sequence, similar to a bidirectionnal RNN. So there is no need to add a specific layer on top of it to mix past and present.

# Marginal probability $p(y_k|\textbf{X})$

In order to marginalize $y_k$, we need to sum over all other $y$ while $y_k$ is fixed. In other words, set the value of $y_{k}$ to 1 to $d$ and let all other $y$ vary. This fixed value is denoted by $y_{k^*}$ below.

$$
\begin{align}
p(y_{k^*}|\textbf{X}) &= \sum_{y_1}\ldots\underbrace{\sum_{y_{k^*-1}}\sum_{y_{k^*+1}}}_{\text{Missing $y_{k^*}$}}\ldots\sum_{y_{K}} p\left(y_1, \ldots, y_{k-1}, y_{k^*}, y_{k+1}, \ldots, y_{K} \big| \textbf{X}\right) \\
&= \frac{1}{Z(\textbf{X})}\sum_{y_1}\ldots\sum_{y_{k^*-1}}\sum_{y_{k^*+1}}\ldots\sum_{y_{K}} \exp\left(
	\sum_k(\textbf{x}_k)_{y_k} + 
	\sum_{k=2}^K V_{y_{k-1}, y_{k}} 
	\right)\\
	
&= \frac{1}{Z(\textbf{X})}\sum_{y_1}\ldots\sum_{y_{k^*-1}}\exp\left(
	\sum_{k=1}^{k^*}(\textbf{x}_k)_{y_k} + 
	\sum_{k=2}^{k^*} V_{y_{k-1}, y_{k}} 
	\right)\sum_{y_{k^*+1}}\ldots\sum_{y_{K}} \exp\left(
	\sum_{k=k^*+1}^{K}(\textbf{x}_k)_{y_k} + 
	\sum_{k=k^*+1}^{K} V_{y_{k-1}, y_{k}} 
	\right)\\
\end{align}
$$

Note that $k^*$ is still in the sum of the exponential because $y_{k^*}$ is still part of the sequence. $y_{k^*}$ is removed from all the outside sums only because it is fixed.

$$
\begin{align}
p(y_{k^*}|\textbf{X}) 
&= \frac{1}{Z(\textbf{X})}\sum_{y_1}\ldots\sum_{y_{k^*-1}}\exp\left(
	\sum_{k=1}^{k^*}(\textbf{x}_k)_{y_k} + 
	\sum_{k=2}^{k^*} V_{y_{k-1}, y_{k}} 
	\right)\sum_{y_{k^*+1}}\ldots\sum_{y_{K}} \exp\left(
	\sum_{k=k^*+1}^{K}(\textbf{x}_k)_{y_k} + 
	\sum_{k=k^*+1}^{K} V_{y_{k-1}, y_{k}} 
	\right)\\
&= \frac{1}{Z(\textbf{X})}\sum_{y_1}\ldots\sum_{y_{k^*-1}}\exp\left(
	\sum_{k=1}^{k^*-1}(\textbf{x}_k)_{y_k} + 
	\underbrace{\sum_{k=1}^{k^*-1} V_{y_{k}, y_{k+1}}}_{\text{Reindex}} 
	\right) \exp(\textbf{x}_{k^*})_{y_{k^*}}
	\sum_{y_{k^*+1}}\ldots\sum_{y_{K}} \exp\left(
	\sum_{k=k^*+1}^{K}(\textbf{x}_k)_{y_k} + 
	\sum_{k=k^*+1}^{K} V_{y_{k-1}, y_{k}} 
	\right)\\
&= \frac{1}{Z(\textbf{X})}
	\underbrace{\sum_{y_1}\ldots\sum_{y_{k^*-1}}\exp\left(
	\sum_{k=1}^{k^*-1}(\textbf{x}_k)_{y_k} + 
	\sum_{k=1}^{k^*-1} V_{y_{k}, y_{k+1}}
	\right)}_{\text{$\alpha(y_{k^*})$: Sum from the left, up to $y_{k^*}$}} 
	\overbrace{\exp(\textbf{x}_{k^*})_{y_{k^*}}}^{\text{ Preference from the model for $y_{k^*}$ }}
	\underbrace{\sum_{y_{k^*+1}}\ldots\sum_{y_{K}} \exp\left(
	\sum_{k=k^*+1}^{K}(\textbf{x}_k)_{y_k} + 
	\sum_{k=k^*+1}^{K} V_{y_{k-1}, y_{k}} 
	\right)}_{\text{$\beta(y_{k^*})$: Sum from the right, up to $y_{k^*}$}}\\
&= \frac{1}{Z(\textbf{X})}
	\alpha\left(y_{k^*}\right)
	\exp(\textbf{x}_{k^*})_{y_{k^*}}
	\beta\left(y_{k^*}\right)\\
&= \frac{\exp\left\{(\textbf{x}_{k^*})_{y_{k^*}} + \log \alpha\left(y_{k^*}\right) + \log \beta\left(y_{k^*}\right)\right\}}{Z(\textbf{X})}
\end{align}
$$


$Z(\textbf{X})$ can also be expressed as 

$$Z(\textbf{X}) = \sum_{y_{k^*}} \exp\left\{(\textbf{x}_{k^*})_{y_{k^*}} + \log \alpha\left(y_{k^*}\right) + \log \beta\left(y_{k^*}\right)\right\}$$


Visually it looks like this: Fix $y_k$ (in blue) and accumulate all the path from the left and the right.

![[forward-backward.svg]]

# Marginal probability $p(y_k, y_{k+1}|\textbf{X})$

With a similar process as above, we end up with

$$
\begin{align}
p(y_{k^*}, y_{k^*+1}|\textbf{X}) 
&= \frac{\exp\left\{(\textbf{x}_{k^*})_{y_{k^*}} + (\textbf{x}_{k^*+1})_{y_{k^*+1}} + V_{y_{k^*}, y_{k^*+1}} + \log \alpha\left(y_{k^*}\right) + \log \beta\left(y_{k^*+1}\right)\right\}}{Z(\textbf{X})}
\end{align}
$$

$Z(\textbf{X})$ can also be expressed as 

$$Z(\textbf{X}) = \sum_{y_{k^*},y_{k^*+1}} \exp\left\{(\textbf{x}_{k^*})_{y_{k^*}} + (\textbf{x}_{k^*+1})_{y_{k^*+1}} + V_{y_{k^*}, y_{k^*+1}} + \log \alpha\left(y_{k^*}\right) + \log \beta\left(y_{k^*+1}\right)\right\}$$

Visually it looks like this. From blue to green is the term $V_{y_{k^*}, y_{k^*+1}}$ in the exponential.

![[forward-backward2.svg]]

# Minimum working example

```python
from datasets import load_dataset, load_metric
from itertools import groupby
import random
from typing import List
from dataclasses import dataclass
import json
from torchsummary import summary
import numpy as np
import pandas as pd
import pickle
import os
import click
import torchcrf
import torch
from transformers import AutoModel, AutoTokenizer
from poutyne import Model, ModelCheckpoint, EarlyStopping


@click.group()
def cli():
    pass


@cli.command()
@click.option("-e", "--epochs", default=10, type=int)
@click.option("-d", "--dataset", default="conllpp", type=str)
@click.option("-c", "--config", default=None, type=str)
@click.option("-lm", default="distilbert-base-cased", type=str)
@click.option("-bs", "--batch-size", default=32, type=int)
def train(epochs, dataset, config, lm, batch_size):
    os.makedirs("ner", exist_ok=True)

    dataset = load_dataset(dataset, config)

    tags = dataset["train"].features[f"ner_tags"].feature
    with open("ner/tags.pkl", "wb") as file:
        pickle.dump(tags, file)

    tokenizer = AutoTokenizer.from_pretrained(lm)

    train_iterator = iterator(dataset["train"], tokenizer, batch_size=batch_size)
    valid_iterator = iterator(dataset["validation"], tokenizer, batch_size=batch_size)

    network = CRF(tags.num_classes)

    summary(network)
    model = Model(network, "sgd", loss_function=loss_func, device=torch.device(1))
    model.fit_generator(
        train_generator=train_iterator,
        valid_generator=valid_iterator,
        epochs=epochs,
        steps_per_epoch=len(dataset["train"]) // batch_size,
        validation_steps=len(dataset["validation"]) // batch_size,
        callbacks=[
            ModelCheckpoint(
                filename="ner/model.pkl", restore_best=True, save_best_only=True
            ),
            EarlyStopping(monitor="val_loss", min_delta=0.0001, patience=10),
        ],
    )


@cli.command()
@click.argument("text")
def predict(text):

    with open("ner/tags.pkl", "rb") as file:
        tags = pickle.load(file)

    network = CRF(tags.num_classes)
    model = Model(network, "sgd", loss_function=loss_func)
    model.load_weights("ner/model.pkl")
    model_checkpoint = "distilbert-base-cased"
    tokenizer = AutoTokenizer.from_pretrained(model_checkpoint)
    tokenized_input = tokenizer(text)
    tags_idx = model.network.decode(
        torch.tensor([tokenized_input["input_ids"]]),
        mask=torch.tensor([tokenized_input["attention_mask"]], dtype=torch.bool),
    )[0]

    tokens = tokenizer.convert_ids_to_tokens(tokenized_input["input_ids"])
    longest_token = max(len(token) for token in tokens)

    return_val = []

    for token, tag_idx in zip(tokens, tags_idx):
        return_val.append(f"{token:{longest_token + 5}}{tags.names[tag_idx]}")

    print("\n".join(return_val))


class CRF(torch.nn.Module):
    def __init__(self, num_tags, model="distilbert-base-cased"):
        super().__init__()
        self.encoder = AutoModel.from_pretrained(model)
        self.emissions = torch.nn.Linear(self.encoder.config.dim, num_tags)
        self.crf = torchcrf.CRF(num_tags, batch_first=True)

    def forward(self, X, tags, mask=None):
        embeddings = self.encoder(X)
        emissions = self.emissions(embeddings.last_hidden_state)
        score = self.crf(emissions, tags, mask, reduction="token_mean")
        return score

    def decode(self, X, mask=None):
        embeddings = self.encoder(X)
        emissions = self.emissions(embeddings.last_hidden_state)
        return self.crf.decode(emissions, mask)


# Poutyne needs to send three inputs to the model and 1 output
# Inputs:
#   1. tokens
#   2. tags (id of the tags, not one hot)
#       The tags will need to be padded to the same length as the tokens
#       Can be padded with 0 or any other existing tags. The CRF layer
#       multiplies the transition value by the mask
#   3. mask
#       0: ignore
#       1: keep
# Output:
#   Poutyne expects a target, so we will have to send None
#   There are no targets, the loss is calculated in the CRF layer
#   Or rather the targets are provided as inputs (tags)
def iterator(dataset, tokenizer, label_pad=0, batch_size=32):
    num_batches = len(dataset) // batch_size + int(len(dataset) % batch_size > 0)
    while True:
        for batch_num in range(num_batches):
            examples = dataset[batch_num * batch_size : (batch_num + 1) * batch_size]

            tokenized_inputs = tokenizer(
                examples["tokens"],
                truncation=True,
                is_split_into_words=True,
                padding=True,
            )

            tags = []
            for i, label in enumerate(examples["ner_tags"]):
                word_ids = tokenized_inputs.word_ids(batch_index=i)
                label_ids = []
                for word_idx in word_ids:
                    # Special tokens have a word id that is None. We set the label to `label_pad`
                    if word_idx is None:
                        label_ids.append(label_pad)
                    # We set the label for the first token of each word.
                    else:
                        label_ids.append(label[word_idx])

                tags.append(label_ids)

            yield (
                torch.tensor(tokenized_inputs["input_ids"]),
                torch.tensor(tags, dtype=torch.long),
                torch.tensor(tokenized_inputs["attention_mask"], dtype=torch.bool),
            ), [None] * batch_size


def loss_func(scores, y_true):
    return -scores


if __name__ == "__main__":
    cli()

```