#### Hash Collision Resistance

Let H: M -> T be a hash function (|M| >> |T|)

A collision for H is a pair of messages that map to the same t.

Obviously with |T| being so much smaller than |H| there will be many collisions
due to the pidgeon hold principle. But H can be collision resistant if there is
no efficient algorithm for finding collisions.

SHA256 is a collision resistant hash function.

Using collision resistant hashing functions is an easy way to convert a MAC for
small inputs into a MAC for large inputs. You can simply hash the message before
both the encrypt and decrypt step.

##### Birthday Paradox Proof

Let r<sub>1</sub>...r<sub>n</sub> from the set {0..B} be independent identically
distributed integers

Theorem: When n = 1.2 x B<sup>1/2</sup> then Pr[two r's are selected that are
the same] >= 1/2

Pr[two different r's are the same] = 1 - Pr[no two r's are the same]
= 1 - Pr[1st r doesn't match]\*Pr[2nd r doesn't match]\* ... \*Pr[nth r matches]
= 1 - Product(i=1 to n-1) (1 - i/B)
< using standard inequality that 1-x <= e<sup>-x</sup> (e<sup>-x</sup>
expands to 1 - x + x<sup>2</sup>/2! - ...) >
>= 1 - Product(i=1 to n-1) (e<sup>-i/B</sup>)
>= 1 - e <sup>Sum(i=1 to n-1) (-i/B)</sup>
>= 1 - e <sup>-1/B * Sum(i=1 to n-1) i</sup>
>= 1 - e <sup>-1/B * (n\*n-1)/2</sup>
>= 1 - e <sup>-1/B * n^2/2</sup>
>= 1 - e <sup>-n^2/2B</sup>
< plugging in the theorem >
>= 1 - e<sup>-0.72</sup>
>= 0.53
> 1/2

##### Merkle-Damgard iterated construction

Break the message up into blocks and run them through a compression function: h:
T x X -> T. T is the previous block or in the case of the initial block, IV.
There is padding or a padding block with is the standard [1000..0 || length of
the message].

##### The Merkle-Damgard paradigm

**Theorem**: If h (compression function) is collision resistant, then so is H
(i.e. this is how you build a collision resistant hash function for large
inputs)

**Proof**:
a collision on H => a collision on h
This is simply because the construction of H is such that the last block
outputing the same hash means a collision for h as well:

Suppose H(M) = H(M') //i.e. colision
then:
IV = H<sub>0</sub>, H<sub>1</sub>, ... , H<sub>t</sub>, H<sub>t+1</sub> = H(M)
IV = H'<sub>0</sub>, H'<sub>1</sub>, ... , H'<sub>r</sub>, H'<sub>r+1</sub> = H'(M')

For this collision to happen, H<sub>t+1</sub> == H'<sub>r+1</sub>

well:
H<sub>t+1</sub> = h(H<sub>t</sub>, M<sub>t</sub> || PB)
H'<sub>r+1</sub> = h(H'<sub>r</sub>, M'<sub>r</sub> || PB')

So there we have two h's that are the same..i.e. a collision. (unless all of
those terms are the same)

Well the last two could be the same.. but if the two H's are the same then we
have another candidate for a collision of h...this logic can be traced back to
the beginning of the message. Either we find a collision for the compression
function h or it turns out that all blocks of M and M' were the same (which is a
contradiction)
