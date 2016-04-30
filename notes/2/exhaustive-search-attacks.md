#### Exhaustive Search Attacks

Goal: given a few input/output pairs (m<sub>i</sub>, c<sub>i</sub> = E(k,
m<sub>i</sub>)) find the key k.

How do we know that this key, k, is unique. Well only one pair is enough to
almost completely constrain a DES cipher:
Lemma: Suppose DES is an **ideal cipher** and there are 2<sup>56</sup> random
invertible functions that map messages to ciphertext. Depending on the key
provided a different function is used. If that were the case, for any m, c pair
there is at most one key such that c = DES(k, m) (this with prob >= 1 - 1/256 or
~99.5%)

(the 1/256 is the probability that DES(k<sup>1</sup>, m) gives the same c as a
given key, k. This is 1/2<sup>64</sup>, or 1 over the number of possible
outputs, summed across the number of keys that you can try, 2<sup>56</sup>,
which yields 2<sup>56</sup> * (1/2<sup>64</sup>) = 1/2<sup>8</sup> = 1/256)

If you had two message ciphertext pairs then the unicity (change of key
collision on those two encryptions) becomes 1 - 2/2<sup>71</sup> or basically
zero. So 2 pairs becomes enough for exhaustive search.

##### DES Challenge
A challenge was issued in 1997 to break DES. Given 3 64 bit messages and their
corresponding ciphertexts, what key was used to encrypt? This challenge took
around 3 months to solve. Since, it's taken much less time, with specialized
machines being built in 1998 that took 3 days to crack DES.

Off the shelf hardware can crack a 56 bit keyspace very quickly..
and 56bit keys therefore should never be used for secure crypto.

To strengthen DES against exhaustive search 3DES was created. You provide 3 keys
and then Encrypt, Decrypt and Encrypt with each key in turn. The decrypt is
hardware hack.. if you use k<sub>1</sub> == k<sub>2</sub> == k<sub>3</sub> which
would result in... regular DES (albeit 3 times slower)

You may think this yeilds the security of a 2<sup>168</sub> bit key. But there
is an attack that brings it down to 2<sup>118</sup> (which is still relatively
secure).

##### Why not double DES??
Exhaustive search for 2<sup>112</sup> takes too long, so it would be secure..

Unfortunately this construction would be very insecure.

With inputs m<sub>1</sub>...m<sub>10</sub> and outputs
c<sub>1</sub>...c<sub>10</sub>

Goal is to find a pair of keys, k<sub>1</sub> and k<sub>2</sub>, that will
satisfy E(k<sub>1</sub>, (E(k<sub>2</sub>, m)) = C
This equation can be refactored: E(k<sub>2</sub>, m) = D(k<sub>1</sub>, c). 
Whenever you can get the two variables you are interested in on separate sides
of the equation typically there is a faster attack. This is called a **meet in
the middle attack**.

What this enables us to do is build a table for the LHS exhaustively and then
calculate the RHS iteratively until it matches the table. To build the table
takes 2<sup>56</sup> then you have to sort it which takes
2<sup>56</sup>log(2<sup>56</sub>) and then you need to calc all the Decryptions
(2<sup>56</sup>) (though on average it will be half of these) and look them up
in the table log(2<sup>56</sup>) for each one. This comes out to:
2<sup>56</sup>log(2<sup>56</sub>) + 2<sup>56</sup>log(2<sup>56</sub>) <
2<sup>63</sup> which is way less than 2<sup>112</sup>

There is a similar attack on 3DES but it still takes 2<sup>118</sup> as
previously stated.
