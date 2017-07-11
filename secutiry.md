Hash flooding attacks

Hashtable lookups are O\(1\) in the best case, but O\(n²\) in the worst case \(i.e. only hash collisions\). Hash flooding attacks trigger the exponential complexity by sending precomputed data, where all keys hash to the same value. If this data somehow gets stored in a hashtable on the server-side, the attack takes effect. A handful of HTTP requests, each sending just a few KB of data, is enough to hang the server CPU for minutes. This attack is possible when the attacker knows the hashing function \(duh, open source\) and the hashing seed used on the server \(whoops!\). Seems that even random seed hash flooding is possible.

