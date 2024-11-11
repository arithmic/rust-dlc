## DLC contract for simple bet between alice and bob, olivia as oracle

Dlc contract between alice and bob with olivia as the oracle. Olivia throws a coin and signs the outcome, which then decides the payout.

Both alice and bob need to send 1 btc to the multisig_address created using alice_pubkey and bob_pubkey. 

Alice creates a multisig_address using her's and bob's pubkey.

Alice and bob transfers some amount to this multisig_address for the contract and takes a note of this txn id.

Alice and bob creates a set of CETs (contract execution transactions) for each outcome that oracle is going to publish.
The signatures are then exchanged and verified by the other party

Alice and bob creates adaptor signature for each of the CETs using the fund txn id, oracle pubkey, oracle r point, and cet (which includes payouts for that outcome). The cets that both alice and bob create should be same.
Each party creates their adaptor signatures independently using their own secret key. 

Alice sends adaptor signatures and CETs so that bob can verify and in turn creates a txn to fund the multisig_address signalling the acceptance of the contract.

After oracle publishes the outcome, alice or bob can use the oracle signature to execute the CET corresponding to the outcome that oracle has published.

### high-level flow rust-dlc sample

bob proposes an offer to alice. 
alice accepts the offer.
    create funding transaction
    dlc contracts also get created. 
    create adaptor signatures for the funding transaction
bob after receiving the acceptance from alice, creates the adaptor signatures for the cets
    create funding transaction
    create adaptor signatures for the cets

## DLC contract for deposit flow: alice, bitvm federation, operator node

create a multisig address for bitvm federation (say using 3 pubkeys member1, member2, member3)
federation_address = multisig_address(member1, member2, member3)
for bitvm bridge
    alice creates a funding transaction (with l2 address in the output) to the multisig_address(alice, federation_address) and sends this as offer to the federation. federation accepts the offer and sends ack to alice. 
    alice after seeing the ack signs the cets to create the adaptor signatures and sends it to the federation. 
    federation after verifying the adaptor signatures of the alice's funding transaction, sends it to the operator node for l2 transaction creation. 
