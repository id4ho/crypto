### Discrete Probability refresher

Modern cryptography was constructed as a rigorous science where constructions
are always accompanied by a proof of security.

#### Probability Distribution

U: for the purposes of this class it will always be a finite set (e.g. U =
{0,1}<sup>n</sup>, which means all combinations of n length strings of 1's and
0's.

A probability distribution, `P`, over a universe, `U`, is the function `P: U -> [0,1]` such that the sum of `P(x)` for all `x`'s in U sum to 1.

__Examples:__
1. Uniform Distribution: for all `x` in `U` the `P(x) = 1/|U|` where `|U|` is
   the number of elements in the universe. i.e. every event `x` is equally likely.
2. Point Distribution at x<sub>0</sub>: P(x<sub>0</sub>) = 1 and
   ∀x≠x<sub>0</sub>: P(x) = 0. Guaranteed to get x<sub>0</sub>

#### Event

A subset of `U`, `A` is known as an event. The probability of the event is
simply the sum of the probability of every element in `A` and it must be between
0 and 1.

#### Union Bound

For events A<sub>1</sub> and A<sub>2</sub> in U, what is the probability that
either one occur?

It's <= the sum of both event's probabilities.

If the two events are disjoint, meaning the probability that both happen is 0,
then the Union will be = the sum of the two event's probabilities.

#### Random Variable

A random variable is a function that maps values from one universe, U, to
a set, V. `X: U -> V`

We say that this set V is where the random variable, X, takes it's values.

More generally we say that a random variable X induces a distribution on V:
Pr[X=v] := Pr[X<sup>-1</sup>(v)] 
The probability that a random sample from the universe will fall into the
preimage of `v` (i.e. map to `v` through `X`) is the same as the probability that a
random sample of `V` will be `v`.

#### Uniform Random Variable

This is a particularly important random variable.

For all `a` in `U` `Pr[r=a] = 1/|U|`.
`r` is formally the identity function: `r(x)=x for all x in U`.

#### Randomized Algorithm

Non-deterministic, will return a different result each time given the same
inputs. The Algorithm is like a Random Variable because it defines a
distribution on all possible outputs.

#### Independence

Events a and b are independent when the knowledge that a happened does not
change the probability that b will happen: `Pr[A and B] = Pr[A] * Pr[B]`

Random variables X,Y taking values in V are independent if for any a,b in V
`Pr[X=a and Y=b] = Pr[X=a] * Pr[Y=b]`

#### Important property of XOR

Theorem: Y, random variable over {0,1}<sup>n</sup>, X, an independent uniform
variable on {0,1}<sup>n</sup> then Z := Y XOR X is uniform variable on
{0,1}<sup>n</sup>

Proof:

for n = 1

| Y|Pr|
| -|--|
| 0|P<sub>0</sub>|
| 1|P<sub>1</sub>|


| Y|X|Pr|
| -|-|--|
| 0|0|P<sub>0</sub>/2|
| 0|1|P<sub>0</sub>/2|
| 1|0|P<sub>1</sub>/2|
| 1|1|P<sub>1</sub>/2|

P[Z=0] =

Pr[(X,Y)=(0,0) or (X,Y)=(1,1)] =
Pr[(X,Y)=(0,0)] + Pr[(X,Y)=(1,1)] = P<sub>0</sub>/2 + P<sub>1</sub>/2 = **1/2**
(since P(0) + P(1) forms an entire universe and must = 1)

