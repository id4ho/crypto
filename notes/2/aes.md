#### AES

AES is built as a substitution-permutation network (not Fiestel, where half the
bits are unchanged round to round) where all the bits are changed every round.

AES schematic: Start with 128 bit block as input which is then organized into a
4x4 matrix where each cell is a byte (16 bytes). This goes through 10 rounds of
being xored with the current round key and having some bit shifting functions
applied: ByteSub, ShiftRow, MixColumn (mix column doesn't happen in last round).
Since the process begins and ends with an xor of a round key there are 11 xors
and only 10 rounds of the bit shifting functions applied. The 128 bit key is
expanded into 11 16 byte keys for each of the xors.

1. ByteSub is essentially a 1 byte lookup table that is applied to each byte in
   the 4x4 matrix.
2. ShiftRow just rotates the rows. row 2 is rotated 1, row 3 -> 2, row 4 -> 3
3. MixColumns linear transformation applied to each of the columns independently
