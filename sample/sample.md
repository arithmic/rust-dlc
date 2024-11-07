# Enum payout example

Trying to simulate a (0-1) cet

## Steps

Run the CLI for both the parties
```bash
cd sample
cargo run ./examples/configurations/alice.yml
```

In a different terminal:
```bash
cargo run ./examples/configurations/bob.yml
```

From the second instance (Bob), type:
```
offercontract <public_key_of_the_alice>@127.0.0.1:9000 ./examples/contracts/enum_contract_input.json
```
Replacing the public key by the one that was displayed when starting the first instance.

From the first instance (Alice), type:
```
listoffers
```
Offer id of the contract that was offered by Bob will be displayed.
A JSON file with contract information should also have been saved in Alice's data folder (`./dlc_sample_alice/offers/xxx.json`).

Alice can now accept the offer by typing:
```
acceptoffer xxxxx
```
replacing `xxxxx` with the contract id that was previously displayed.

This will make Alice's instance send an accept message to Bob.

In Bob's instance, typing `listcontracts` will trigger the processing of the message, and the contract will be `Signed`, as Bob will automatically reply to Alice with a `Sign message`.

Typing the same command (`listcontracts`) in Alice's instance will make Alice broadcast the fund transaction.

Now open the terminal in arithmic-1 (server where the bitcoin testnet is running):
```bash
bitcoin-cli -regtest -rpcwallet=alice getnewaddress bec32
bitcoin-cli -regtest generatetoaddress 10 <new_address>
```
Replace the `new_address` with the address generated in the previous command. This will generate some blocks so that the fund transaction is confirmed.

Typing `listcontracts` in either instance should now show the contract as `Confirmed`.

Fetch the transaction details using funding transaction hash. The bridge transaction details will be included in hex field of the response

```bash
curl --location 'http://14.143.177.166:18443/wallet/alice' \
--header 'Content-Type: application/json' \
-u 'arithmic:arithmic' \
--data '{
  "jsonrpc": "1.0",
  "id": "get transaction details",
  "method": "gettransaction",
  "params": ["<funding_transaction_hash>"]
}'
```