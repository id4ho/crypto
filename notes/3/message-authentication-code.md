#### Message Authentication Code (MACs)

One example of MAC is a two algorithm integrity system, I = (S,V) where S(k, m)
outputs a tag, t, in T and V(k, m, t) outputs 'y' or 'n'.

Consistency requirement: for any key, k, in K and any message, m, in M it must
hold that V(k, m, S(k, m)) = 'y'

Integrity **requires** a shared key between alice and bob.

The classic checksum algorithm, cyclic redundancy check or CRC, is only meant to
detect random errors, not malicious ones.

To evaluate the security of MACs we use the following paradigm:

- **attack**: chosen plaintext attack
- **goal**: existential forgery (produce some new valid message/tag pair) or
  find another tag, t', that works for one of the m,t pairs.

##### Protecting system files on disk

Suppose at install the computer asks the user for a password. It then takes this
password and adds a tag to every system file.

If later a virus infects the system and modifies system files, the user can find
out which system files were changed by logging into a clean OS providing his
password and checking all the tags.

\* caveat: the virus could swap files in this case without detection (since the
m,t would still be a valid pairs for each. So the OS would need to include the
file name/path in the message used for tag generation.
