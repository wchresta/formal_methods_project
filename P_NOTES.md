

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

