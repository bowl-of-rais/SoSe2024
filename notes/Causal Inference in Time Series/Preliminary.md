# Probability

## Probability Spaces

**probability space**: triple $(\Omega, \mathcal{F}, P)$
- $\Omega$: set of possible *outcomes* of random event
- $\mathcal{F}$: set of subsets of $\Omega$ (= *events*). abstraction: [Borel algebra](https://en.wikipedia.org/wiki/Borel_set), $\sigma$-algebra
- $P$: probability function, $\mathcal{F} \to [0, 1]$

<mark style="background: #ADCCFFA6;">axioms for $\sigma$-algebra:</mark>
- $\Omega \in F$
- $\mathcal{A}\in F \rightarrow \mathcal{A}^c F$
- $A_1, \dots, A_n \in \mathcal{F} \rightarrow \bigcup A_i \in \mathcal{F}$ aka closed under union operation

<mark style="background: #ADCCFFA6;">axioms for probability function:</mark>
- $P(\Omega)= 1$
- $P(\cdot) \geq 0$
- for $A, B$ mutually exclusive: $P(A \cup B) = P(A) + P(B)$

*mutually exclusive events*: $A \cap B = \emptyset$
##### Example: checking axioms
$$P(\lceil a, b \rceil) \triangleq b - a, \quad 0 \leq a \leq b \leq 1$$
1. $P(\lceil 0, 1\rceil) = 1 - 0 = 1$
2. $P(\lceil a, b\rceil) = b - a \geq 0$
3. $P(\lceil a_{1}, b_{1} \rceil \cup \lceil a_{2}, b_{2} \rceil) = P(\lceil a_{1}, b_{2} \rceil) - P(\lceil b_{1}, a_{2}) = (b_{2} - a_{1}) - (a_{2} - b_{1}) = (b_{2} - a_{2}) + (b_{1} - a_{1})$
##### Theorem
$$\text{for } B_{1} \subseteq B_{2} \subseteq \dots: \lim_{i \to \infty} P(B_{i}) = P\left(\bigcup_{i=1}^{\infty}B_{i} \right)$$proof via definition of mutually exclusive sets
## Independence
$$P(A \cap B) = P(A) \cdot P(B) \Rightarrow A \perp \!\!\! \perp B$$
##### Example
TODO
## Bayes' Rule

**conditional probability**: $P(A \mid B) \triangleq \frac{P(A \cap B)}{P(B)}$

Bayes' Rule $$P(A \mid B) \cdot P(B) = P(B \mid A) \cdot P(A)$$
## Total Law of Probability
$$P(E) = \sum\limits_{i=1}^{m} P(A_{i} \cap E) = \sum\limits_{i=1}^{m} P(E \mid A_{i}) P(A_{i})$$
## Causal Relationships
- if there is a causal relationship between events, they cannot be independent and vice versa
## Random Variables
$$X : \Omega \to \mathbb{R}$$
- *cumulative density function*: $CDF = P(X \leq x) = F(x)$
- *probability density function*: $PDF = \mathcal(F)(x) = \frac{dF}{dx}$
- *probability mass function*: $P(X = x) = P(x)$
	- $\sum\limits_{i=1}^{N} P(X=i) = 1$
#### Normal distribution
$$\mathcal{F}_{N}(x) = \frac{1}{\sqrt{2 \pi \sigma²}}e^{\frac{-(x-\mu)^2}{2\sigma^2}}$$
#### Mean and Variance of random variables
- *mean*: $E[X] = \int_\mathbb{R} x \mathcal{F}(x) dx = \int_\mathbb{R} x d F(x)$
- *variance*: $E[(X - E[X])^{2]}= \int (x-\mu)^{2}\mathcal{F}(x) dx$
# Random/Stochastic processes



# Linear Algebra



# Graph Theory
- causal relationships