
== Assignment 2.1 ==

We justify correctness of the OTR key exchange informally.
TODO: Correctnes of which property? What do they mean with correctness?

== Assignment 2.3 ==

=== Injective agreement for initiator ===
Injective agreement for initiator was disproven since an actor can be tricked
into taking role R when he thinks he's I:

- A wants to talk to B in the role of the initiator
    A(I)-> : <X, sign{<X,B>}ltkA, pk(ltkA)>
- B wants to talk to A in the role of the initiator
    B(I)-> : <Y, sign{<Y,A>}ltkB, pk(ltkB)>
- A recieves the initiating request from B
    A(I)<- : <Y, sign{<Y,A>}ltkB, pk(ltkB)>
    A(I) finishes the run, but B was never in role R

From the perspective of A, he has now a channel with B where A is the initiator
and B is the reciever. But was never in this role.

This can even happen by chance when both A and B want to initiate a connection
and both first messages arrive at the same time. In this case, the answer message
looks like the initiator message.

=== Injective agreement for reciever ===
From the recievers point of view, injectiveness does not hold trivially: If both
reciever and initiator want to agree on g^{xy}, then the initiator has to recieve
Y=g^y before he can commit to g^{xy}. This can only happen AFTER the reciever committed
to g^{xy}. The attacker can just block the last (2nd) message and so the initiator will
never commit to g^{xy}. This breaks injective agreement for the reciever.

It looks as follows:
    A(I)->B(R) : <X, sign{<X,B>}ltkA, pk(ltkA)>
    B(R)->     : <Y, sign{<Y,A>}ltkB, pk(ltkB)>
    B(R) commits to g^{xy}
    A(I) never commits since it never recieves the last message


== Assignment 2.4 ==

The protocol is the following
                                                                                                
    A -> B: g^x
    A <- B: g^y                         
    A -> B: <A, sign{<g^x,g^y>}ltkA, mac{<"0",A>}hash(g^xy), pk(ltkA)>
    A <- B: <B, sign{<g^x,g^y>}ltkB, mac{<"1",B>}hash(g^xy), pk(ltkB)>

We have looked at this protocol for two cases. Including the possibility
of agents talking to themselves and where we excluded this possibility.
In the case where we allowed agents to initiate communications with themselves,
we were able to break non-injective agreement for both cases (initiator and reciever).
We stored the proofs as "OTR4-proof.spthy".

In the case where we excluded agents initiating with themselves, we were able
to prove secrecy, PFS, injective agreement for the initiator. We also found
the same attack as before to break non-injective agreement for the reciever.
We stored the proofs as "OTR4-proof-noteq.spthy"

We differentiate between these two cases using a restriction.

=== Non-Injective agreement for initiator when A=B ===
We found an attack on A which looks like follows:

    A(I)-> : g^x
    A(I)<- : g
    A(I)-> : <A, sign{<g^x,g>}ltkA, mac{<"0",A>}hash(g^x), pk(ltkA)>
    A(I)<- : <A, sign{<g^x,g>}ltkA, mac{<"1",A>}hash(g^x), pk(ltkA)>

This makes A talk to itself with a publicly known key. A will commit
to K=g^x without there ever beeing in the reciever role. Since K=g^x=X,
this also breaks secrecy.

If we exclude this (rather uninteresting) case of A talking to itself,
we were able to prove non-injective agreement for the initiator.  
We were also able to prove perfect forward secrecy in this case.

=== Non-Injective agreement for reciever ===
Even when we exclude the case A=B, we were able to find an attack for injective
agreement on the reciever. The attack is the following (stored in OTR4-proof-noteq.spthy):

In the prooffile, we have the correspondance: A->$A.1, B->$A, C->$A.2
A will initiate a connection with C, B will recieve a connection from A.

    A(I)-> : g^x // Meant for C
    B(R)<- : g^x
    B(R)-> : g^y
    A(I)<- : g^y
    A(I)-> : <A, sign{<g^x,g^y>}ltkA, mac{<"0",A>}hash(g^xy), pk(ltkA)> // Meant for C
    B(R)<- : <A, sign{<g^x,g^y>}ltkA, mac{<"0",A>}hash(g^xy), pk(ltkA)>
    B(R)-> : <B, sign{<g^x,g^y>}ltkB, max{<"1",B>}hash(g^xy), pk(ltkB)>

B has now committed to K=g^xy as a shared secret with A, but A never run the protocol
with B. This contradicts non-injective agreement for the reciever.
The reason why this attack is possible is, because none of the messages contain the
intended recepient.


