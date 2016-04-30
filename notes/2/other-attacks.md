#### Other attacks

1. Side channel attacks:
  - Measure time taken to do enc/dec, measure power for enc/dec
2. Fault attacks:
  - Overclocking the processor or overheating it can cause it to error. If there
    are computing errors in the last round the secret key k may be fully exposed
3. Linear and Differential attacks:
  - given many input output keys, we can recover the key in less than
2<sup>56</sup>. Essentially a subset of the message bits xored with a subset of
the ciphertext bits will yeild a subset of the key bits with greater probability
than 1/2 + epsilon. A tiny bit of linearity in the 5th s-box of DES leads to an
attack of this nature which requires a lot of input (2<sup>42</sup>) but leads
to an attack that only takes 2<sup>42</sup> (s.i.c.)
4. Quantum attacks
  - A generic search problem is f: X -> {0,1} be a function, Goal is to find x
    from within X that will return f(x)=1 (where only one x returns 1 and every
other output is 0.
  - Classical takes O(|X|) whereas Quantum would take O(|X|<sup>1/2</sup>) (per
    Grover's algorithm)
  - If you had a quantum computer, the time required for exhaustive search would
    be cut in half.
