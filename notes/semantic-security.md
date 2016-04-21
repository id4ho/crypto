#### Semantic Security

What is a secure cipher?
The CT should reveal no "info" about the PT

Semantic security when the attacker only has one CT:

For b = 0 and b = 1, we define experiments EXP(0) and EXP(1)
The adversary, A, will submit two messages, m<sub>0</sub> & m<sub>1</sub> that
are of equal length to the challenger. The challenger will return to A the CT,
c <- E(k, m<sub>b</sub>), depending on whether it was input b = 0 or b = 1.

A will then output, b', which is whether it thinks it was b = 0 or b = 1.

W<sub>0</sub> := [the event that EXP(b) = 1]
Similarly, W<sub>1</sub> := [the event that EXP(b) = 1]

Adv<sub>ss</sub>\[A, E\] (the semantic security advantage of the adversary, A, against
the scheme E)

Adv<sub>ss</sub>[A,E] := |Pr[W<sub>0</sub>] - Pr[W<sub>1</sub>]|

We are interested in whether the adversary outputs 1.. if the adversary outputs
1 with different probabilities then it can distinguish between the two CTs.

##### OTP semantic security

For all A (we don't even need to restrict A to efficient A's) OTP is
semantically secure because the distributions of k XOR m<sub>0</sub> and k XOR
m<sub>1</sub> are identical.

Adv<sub>ss</sub>[A, OTP] = |Pr[A(k XOR m<sub>0</sub>) = 1] - Pr[A(k XOR
m<sub>1</sub>) = 1]| = 0

##### Stream ciphers semantic security

Theorem: G:K -> {0.1}<sup>n</sup> is a secure PRG => stream cipher E derived
from G is semantically secure.

For any sem. sec. adversary A, there exists a PRG adversary B s.t.
Adv<sub>ss</sub>[A,E]  <= 2 * Adv<sub>PRG</sub>[B,G]

the Adv<sub>PRG</sub> is negligible (since the PRG is secure)

Adv<sub>ss</sub>[A,E] = |Pr[W<sub>0</sub>] - Pr[W<sub>1</sub>]|

claim 1: |Pr[R<sub>0</sub>] - Pr[R<sub>1</sub>]| = Adv<sub>ss</sub>(A, OTP) = 0

claim 2: there exists a B such that |Pr[W<sub>b</sub>] - Pr[R<sub>b</sub>]| =
Adv<sub>PRG</sub>(B,G)

|---------|---------------|-------------------|------------------|
0       Pr[W<sub>0</sub>]    Pr[R<sub>b</sub>]    Pr[W<sub>1</sub>]         1

These two claims show:
Adv<sub>ss</sub>[A,E] = |Pr[W<sub>0</sub>] - Pr[W<sub>1</sub>]| <= 2 * Adv<sub>PRG</sub>[B,G]

We know the first claim is true, why is the second claim true
