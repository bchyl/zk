# privately and securely bridge 100,000 UST tokens from Ethereum to Harmony
A set of smart contracts deployed on Ethereum and Harmony chains. When token locking activity is detected on the initial chain, the verifier pool verifies the transaction and passes the final information to the target chain, which in turn generates an equal number of bridging tokens 1:1.

Ethereum-to-Harmony (transfer assets) flow:

- A user requests the bridge to transfer ERC20s and provides the Harmony address to receive 1:1 HRC20s on Harmony, by authorizing the bridge which request `EthManager`.
- The bridge hosts a set of validators who listen to this request, perform lock, wait for enough block confirmations on Ethereum to ensure finality.
- Upon confirmation, bridge validators request `HmyManager` to mint the HRC20 assets and transfer to recipient user account.
[transfer assets flow](https://miro.medium.com/max/1400/1*E_Gj0Skg4y2Zxa0-y3QqWg.jpeg)

## challenges & potential resolution
- Reduce the cost of header validation: Header validation for light clients is expensive.
reducing these costs can bring us closer to fully universal and trust-free interoperability. An interesting design might be to bridge to L2.
- a Trusted model ->  Bonded model -> Insured model: Bound validators and Repeaters can suppress malicious behavior, but the protocol is not enough.

# private bridge (specifically the withdrawal methods) to prevent a similar attack
- Deal with reward settlement before credential transfer, and record user reward cache before and after transfer.
- More nodes distributed in more community-run or third-party projects, plus there are multiple software clients. Increase the number of verifiers to prevent multiple signature attacks.
- Using light client relay Cross chain communication protocol, such as cosmos light client IBC, Near Rainbow Bridge.
- Using Trustless, we can reduce some potential attack surfaces to improve system security, Mature Rollup cross chain bridge.
- Through a much more secure centralized exchange, the native assets of the corresponding chain are exchanged and then extracted into the corresponding chain to avoid possible smart contract risks.
