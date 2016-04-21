### Information Theoretic Security

__Basic idea (Shannon 1949)__ CT should reveal no info about the PT

Formally, what does "info" mean?

Definition: A cipher (E,D) over (K,M,C) has perfect secrecy if for any two
messages in the messagespace (same length) the `Pr[E(k, m0) = c]` is equal to
`Pr[E(k, m1) = c]` and `k` is uniform in `K`.

Given CT can't tell if msg is `m0` of `m1`, even the most powerful adversary
learns nothing about PT from the CT. __NO CT only attack!__

OTP (one time pad): Secure cipher that involves a keyspace that is the same size
as the message space and is actually secure (assuming truly random pad)

__Lemma__: OTP has perfect secrecy
__Proof__: For any `m,c` the `Pr[E(k, m) = c]` is equal to the number of keys in
the keyspace that will encrypt `m` to `c` (`E(k, m) = c`) divided by the total
number of keys in the keyspace.
So if the number of keys that will encrypt `m` to `c` (`#{k in k: e(k, m) = c}`)
is constant then the cipher has perfect secrecy.

The number of keys that will encrypt a message to a certain ciphertext using the
OTP is 1, therefore it has perfect secrecy.

The bad news: to have perfect secrecy, K must be >= M. Which is impractical
because the key length must be >= the message length.
