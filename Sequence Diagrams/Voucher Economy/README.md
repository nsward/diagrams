# Voucher Economy Sequence Diagram

![diagram](http://www.plantuml.com/plantuml/proxy?src=https://raw.githubusercontent.com/nsward/diagrams/develop/Sequence%20Diagrams/Voucher%20Economy/voucher-economy.puml)

## Additional Notes

### Blockchain Networks
There are two (potentially) separate blockchains within the system shown in the diagram. Both will be private forks of the Ethereum blockchain using Proof of Authority for consensus. The Disberse Blockchain supports the Disberse platform and any additional services built on top of the platform.


The Voucher Economy also needs access to an EVM-compatible (Ethereum Virtual Machine) blockchain, but the structure of that blockchain requires more research and consideration. It's feasible that the Voucher smart-contracts could leverage the existing validator set and network properties from the Disberse blockchain. It also may be preferred to run a separate network validated by a constant set of entities (i.e. permanent members of the 121 project). A third option would be to have the organizations that are currently using the 121 code become the validators (i.e. everyone who runs an instance of 121 also runs a validator node).

#### Communicating with the blockchain
This depends slightly on the structure of the network and whether or not the caller is running a validator node. But, in either case calls are made using JSON-RPC, specifically `eth_sendRawTransaction` for sending signed state-changing transactions and `eth_call` for querying data from a single node. More info on the JSON-RPC API can be found [here](https://github.com/ethereum/wiki/wiki/JSON-RPC).

#### Funding Service Smart-Contracts
The funding service talks to the larger ecosystem of Disberse smart-contracts, but there are two relevant contracts to discuss.
1. A token contract, which stores user balances and provides functions for querying or updating those balances, including the following:
    - `balances(userAddress)` - returns the balance of the user
    - `burn(amount)` - deducts `amount` of tokens from sender's balance
    - `transfer(dstAddress, amount)` - deducts `amount` of tokens from sender's balance and adds to balance of `dstAddress`
2. An exchange logging contract, which allows Disberse to log the exchange rates received if funds are exchanged into a different currency after leaving the Disberse platform. For example, if funds held in Euros on the Disberse platform are exchanged into Kwacha in transit to an external bank account, that exchange rate is recorded on-chain via the exchange logging contract.


In order to determine the funds that are available for a humanitarian organization to distribute, the funding service provides a wrapper for the user's banking provider. In this case, the wrapper allows the user to enter their Disberse account number and view the funds that have been redeemed from the Disberse platform (meaning they are either on their way or already held with the FSP).


#### Voucher Service Smart-Contracts
The voucher service consists of one main smart contract. The voucher contract is a token contract similar to that discussed above, but currently excluding the `transfer` function (as PAs cannot currently transfer their vouchers to others).
