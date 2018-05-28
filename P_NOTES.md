

== P2a ==

We implemented the following protocol

     A -> B: x
     A <- B: y
     A -> B: {y}_{k(A,B)}
     A <- B: {x}_{k(B,A)}

And found attacks on both non-injective for the initiator and the receiver.

=== Non-Injective agreement on initiator ===
In the attack on the non-injective initiator, we can make two initiators talk
to eachother.

         <-B(I) : y  {dropped}
     A(I)->B(I) : x
     A(I)<-     : y' {injected}
     A(I)->     : {y'}_{kAB} {dropped}
     A(I)<-B(I) : {x}_{kBA}

This finishes the run for A, thinking it talks to B but B was never in role R.
Also, A committed to value y' which B never had.


=== Non-Injective agreement on reciever ===
In this attack, we can make the reciever commit to a wrong value:

         <-A(I) : x  {dropped}
     B(R)<-     : x' {injected}
     B(R)->A(I) : y
     B(R)<-A(I) : {y}_{kAB}
     B(R)->     : {x'}_{kBA} {dropped}

This finishes the run for B in role reciever, committing to x' which A never had.

== P2b ==
We fix the protocol problems of P2a by including both nonces in both mac's. We also
include the role of the actors to make sure reflexion attacks are mitigated.

This leads us to the following protocol:

     A -> B: x
     A <- B: y
     A -> B: {<'I',x,y>}_{k(A,B)}
     A <- B: {<'R',x,y>}_{k(B,A)}

We were able to prove injective agreement on x and y for both parties.

== P3a ==
We implemented the described protocol and were able to prove injective agreement for
both parties on x, y and the session key sIR, as well as secrecy for sIR.
We disproved perfect forward secrecy, but are not giving details for this attack
since it is not part of the assignment.

=== (b) ===
We informal reason why we can get rid of both nonces in the reply is, that the
information about both nonces is part of the session key sIR. If either of the
parties would not agree on the same nonces, the session key would be derived
differently. In fact, it would probably be enough to apply the mac to a fixed
string mac{'I'}sIR. We did not check this.



