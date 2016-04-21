### PseudoRandom Generators

Function that takes a seed in the seed space `S` and maps it into
a __much__ larger string `N`.

Must be efficiently computable and deterministic and the output must "look"
random.

##### Stream Cipher
An encryption stream that uses a random seed, `k` and a PRG to generate a random
sequence to xor with the PT to gain a secure CT.

##### Why must the PRG be unpredictable?

If it is predictable, the following is true:

There exists an `i` that G(k)|<sub>1,..,i</sub> -> G(k)|<sub>i+1,..,n</sub>
(where G(k)|<sub>sub</sub> means that given `sub` elements of G(K))

If you know that the message is being sent via SMTP and the headers are
`FROM:xyz TO:abc`, then from the ciphertext only you can determine the G(k) for
the first few bytes. If the PRG is predictable then you can determine the full
G(k) and compute the full `m`.

##### What defines predictable?
G:K -> {0,1}<sup>n</sup> is predictable if there is an efficient algo, A, a
number, i, 1 <= n <= n-1 such that:

Pr[A(G(k))|<sub>1,...,i</sub> = G(k)|<sub>i + 1</sub>] >= 1/2 + epsilon

\*for some non-negligible epsilon (> 1/2^30)

In practice, epsilon, is scalar:

non-neg: >= 1/2^30
neg: <= 1/2^80

In theory, it is a function that outputs positive real values (probabilities):

non-neg: there exists `d` such that epsilon(l) >= 1/l^d
(if something is often (infinitely often) bigger than 1/polynomial)

__examples:__
`epsilon(l) = 1/l^1000` (even though this is very tiny, if we set `d` to be
10,000 then this function is clearly > 1/l^10000. So it's larger than some
polynomial fraction.) This is polynomially small.

neg: given any `d` (and l >= l<sub>d</sub>) epsilon(l) <= 1/l^d
(for all d, there exists some lower-bound l<sub>d</sub> where the function is
smaller than 1/polynomial. Negligible if the function is less than
1/l<sub>d</sub> for sufficiently large l)

__examples:__
`epsilon(l) = 1/2^l` (this is because for any constant, `d`, there is
sufficiently large `l` such that `1/2^l < 1/l^d`) This is exponentially small.

#### What does it mean to be indistinguishable from random?

##### Statistical Tests
An algorithm, A, s.t. A(x) outputs "0" if not random and "1" if random
Examples:
1. A(x) = 1 iff |#0(x) - #1(x)| <= 10 * sqrt(n)
2. A(x) = 1 iff |#00(x) - n/4| <= 10 * sqrt(n)
3. A(x) = 1 iff longest-run-of-0(x) <= 10 * log<sub>2</sub>(n)

In the old days, you'd take a lot of these statisticals tests and say that the
generator is random if they all return 1.

##### Advantage
Adv<sub>PRG</sub> [A, G] := |Pr[A(G(k)) = 1] - Pr[A(r) =1]|
Adv close to 1 => A can distinguish G from random
Adv close to 0 => A cannot distinguish G from random

##### Crypto definition of a secure PRG
For any efficient statistical test, A, Adv<sub>PRG</sub>[A, G] is "negligible".
Are there provably secure PRGs? Unknown.. if you could prove there was one, you
would prove P!=NP. Or if you could prove that P=NP we would know that there are
NO provable secure PRGs.

Yao (1982) an unpredictable PRG is secure. Vice versa: if a PRG is insecure, it
is predictable.



