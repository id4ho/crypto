#### TLS record protocol

Each record is max 16kB. One key is used to send messages from brower to server
and one from server to broswer. Both sides know both keys, k<sub>b->s</sub> and
k<sub>s->b</sub>.

Each side maintains a 64 bit counter, set to 0 when connection is established.
(this serves as replay defense)

In older versions of TLS the IV was predictable. IV of next record was the last
block of the previous record. This is not CPA secure.


