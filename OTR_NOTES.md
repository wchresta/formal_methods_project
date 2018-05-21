
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


