# Tuxedo

- **Team Name:** Off-Narrative Labs
- **Payment Address:** 0x5a335908df9D2C47304338E3b744579Ed7C6a64d (USDT)
- **[Level](https://github.com/w3f/Grants-Program/tree/master#level_slider-levels):** 2 :baby_chick:

## Project Overview :page_facing_up:

Develop Substrate runtimes based on the UTXO model.

### Overview

Tuxedo is a framework for developing Substrate runtimes based on the Unspent Transaction Output (UTXO) model. The letters utxo are contained in the word tuxedo, hence the name.

In the broader blockchain space, there are essentially two models for creating state machines, or runtimes, as they are known in the Substrate ecosystem. Those two models are the **Account System** as seen in Ethereum, Polkadot, and others, and the **UTXO System** as seen in Bitcoin, Monero, Cardano, and others. Currently the defacto way to write Substrate runtimes is with FRAME, which is based on the account system, and any project wishing to build on the utxo system is left to write a runtime from scratch or find a home in another ecosystem. Tuxedo would be a couterpart to FRAME based on UTXOs rather than accounts.

Tuxedo would make the UTXO model more visible and accessible in the Substrate ecosystem and begin to create a diversity of runtime frameworks in addition to FRAME, a trend that we hope will continue beyond Tuxedo itself.

### Project Details

The primary advantage of UTXOs is that they are highly parallelizable. This fits well in Polkadot's multichain ecosystem where parachains execute and communicate asynchronously, and will be an even bigger advantage if (hopefully when) DAG based chains become popular, a trend that is already kicked off with projects like Aleph Zero, and many others outside the Polkadot ecosystem, including Hedera Hashgraph, Nano, and Casper Labs.

The team has done previous work on this topic.
* Joshy [maintained](https://github.com/JoshOrndorff/utxo-workshop/tree/joshy-update-deps-may-2022) the Substrate UTXO Workshop code as part of the Substrate Developer Hub team, and has continued to maintain it out of personal interest even years after leaving that team.
* Andrew [ported](https://github.com/coax1d/utxo-frameless/) this code to work _without FRAME_ as part of the Polkadot Blockchain Academy.

The UTXO data model is relatively well established by Bitcoin as well as research from IOHK their [Abstract Model](https://eprint.iacr.org/2018/469.pdf)  and [Extended UTXO Model](https://files.zotero.net/eyJleHBpcmVzIjoxNjc1MjAwMTAwLCJoYXNoIjoiYTVhYmY4NjdiY2E2YzdkNTNjODkwNWNmZDZhYmM5MjAiLCJjb250ZW50VHlwZSI6ImFwcGxpY2F0aW9uXC9wZGYiLCJjaGFyc2V0IjoiIiwiZmlsZW5hbWUiOiJDaGFrcmF2YXJ0eSBldCBhbC4gLSAyMDIwIC0gVGhlIEV4dGVuZGVkIFVUWE8gTW9kZWwucGRmIn0%3D/ddc74b205ca4890fe1d87770bee15dd5a82bfed1ad8f84217cbf407686958498/Chakravarty%20et%20al.%20-%202020%20-%20The%20Extended%20UTXO%20Model.pdf). Our primary tasks would be to implement this in Rust and expose a standard API for chain developers to build on. This is analogous to the API exposed by FRAME System and the Pallets built on top.

Our core data types follow similarly, to the IOHK research cited above. The primary differences are that we do not assume a native cryptocurrency and rely on Tuxedo **Pieces** (analogous to FRAME Pallets) to provide the validation logic, rather than the UTXOs themselves.

```rust
/// A UTXO transaction specifies some inputs to be consumed, and some new outputs to be created.
struct Transaction {
  /// The inputs refer to currently-existing unspent outputs that will be consumed by this transaction
  inputs: BTreeSet<Input>,
  /// The new outputs to be created by this transaction.
  outputs: Vec<Output>,
}

/// A single output of a transaction which has an owner and some associated data
struct Output {
  /// The address that owns this output. Based on either a public key or a Tuxedo Piece
  owner: Address,
  /// The data associated with this output. In the simplest case, this will be a token balance, but could be arbitrarily rich state.
  data: Vec<u8>,
}

/// A single input references the output to be consumed and provides some witness data, possibly a signature.
struct Input {
  /// A previously created output that will be consumed by the transaction containing this input.
  output: OutputId,
  /// A witness proving that the output can be consumed by this input. In many cases including that of a basic cryptocurrency, this will be a digital signature.
  redeemer: Vec<u8>,
}
```

The core of the API exposed developers who create Tuxedo Pieces, will roughly follow this trait. We expect this will have to get more specific as our development shows us what we haven;t yet considered.

```rust
/// The API of a Tuxedo Piece
trait TuxedoPiece {

  /// The type of data stored in Outputs associated with this Piece
  type Data;

  /// The validation function to determine whether a given input can be consumed.
  fn validate(transaction: Transaction, input: Input) -> bool;
}
```

This grant does not strive to create the entire rich ecosystem of Tuxedo pieces that we hope to eventually be developed on top of Tuxedo. Rather it strives to create the core of the Tuxedo system and a few of the most important and exemplary pieces. Specifically, we strive to develop the analogs to FRAME Executive, FRAME System, Pallet Balances, and Pallet Transaction Payment.

### Ecosystem Fit

Tuxedo is a framework for writing Substrate runtimes. Substrate is the toolkit for building virtually all parachain nodes as well as many standalone blockchains. As such, Tuxedo provides a richer set of options to runtime developers, and hopes to attract teams to the substrate / Polkadot ecosystem who may have gone elsewhere.

The primary users of Tuxedo will be parachain and runtime developers who will use Tuxedo directly to structure their runtimes. Of course, the user base will trickle downstream as well to users of those parachains that choose to build with Tuxedo. However, chain users will use Tuxedo only indirectly.

There are no projects like this in the Substrate ecosystem, although they do exist in the broader blockchain space; Cardano being the most notable example.

## Team :busts_in_silhouette:

### Team members

- Joshy Orndorff https://github.com/JoshOrndorff
- Andrew Burger https://github.com/coax1d

### Contact

- **Contact Name:** Joshy Orndorff
- **Contact Email:** joshyorndorff at proton dot me



### Team's experience

Joshy entered the Substrate ecosystem in 2019 as part of the Substrate Developer Hub team. There he created and hosted the weekly Substrate Seminar, and contributed significantly to the Substrate Recipes. In 2020, he moved to the Moonbeam team where he was a core developer. While at Moonbeam, Joshy wrote the [Nimbus consensus engine](https://github.com/PureStake/nimbus/) which is used in several production parachains, and helped pioneer the technique whereby EVM contracts can interact with native Substrate pallets. In 2022, Joshy began contributing to the Polkadot Blockchain Academy, teaching in both Cambridge and Buenos Aires.

Andrew entered.... TODO Andrew ....

Joshy and Andrew met in Cambridge in 2022 at the first Polkadot Blockchain Academy. There Andrew chose the Frameless UTXO Project cited above as his final project.


## Development Status :open_book:

TODO

If you've already started implementing your project or it is part of a larger repository, please provide a link and a description of the code here. In any case, please provide some documentation on the research and other work you have conducted before applying. This could be:

- links to improvement proposals or [RFPs](https://github.com/w3f/Grants-Program/tree/master/docs/RFPs) (requests for proposal),
- academic publications relevant to the problem,
- links to your research diary, blog posts, articles, forum discussions or open GitHub issues,
- references to conversations you might have had related to this project with anyone from the Web3 Foundation,
- previous interface iterations, such as mock-ups and wireframes.

## Development Roadmap :nut_and_bolt:

TODO... Lots more TODO. This is long.

This section should break the development roadmap down into milestones and deliverables. To assist you in defining it, we have created a document with examples for some grant categories [here](../docs/Support%20Docs/grant_guidelines_per_category.md). Since these will be part of the agreement, it helps to describe _the functionality we should expect in as much detail as possible_, plus how we can verify and test that functionality. Whenever milestones are delivered, we refer to this document to ensure that everything has been delivered as expected.

Below we provide an **example roadmap**. In the descriptions, it should be clear how your project is related to Substrate, Kusama or Polkadot. We _recommend_ that teams structure their roadmap as 1 milestone ≈ 1 month.

> :exclamation: If any of your deliverables is based on somebody else's work, make sure you work and publish _under the terms of the license_ of the respective project and that you **highlight this fact in your milestone documentation** and in the source code if applicable! **Teams that submit others' work without attributing it will be immediately terminated.**

### Overview

- **Total Estimated Duration:** Duration of the whole project (e.g. 2 months)
- **Full-Time Equivalent (FTE):**  Average number of full-time employees working on the project throughout its duration (see [Wikipedia](https://en.wikipedia.org/wiki/Full-time_equivalent), e.g. 2 FTE)
- **Total Costs:** Requested amount in USD for the whole project (e.g. 12,000 USD). Note that the acceptance criteria and additional benefits vary depending on the [level](../README.md#level_slider-levels) of funding requested. This and the costs for each milestone need to be provided in USD; if the grant is paid out in Bitcoin, the amount will be calculated according to the exchange rate at the time of payment.

### Milestone 1 Example — Basic functionality

- **Estimated duration:** 1 month
- **FTE:**  1,5
- **Costs:** 8,000 USD

> :exclamation: **The default deliverables 0a-0d below are mandatory for all milestones**, and deliverable 0e at least for the last one. 

| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| **0a.** | License | Apache 2.0 / GPLv3 / MIT / Unlicense |
| **0b.** | Documentation | We will provide both **inline documentation** of the code and a basic **tutorial** that explains how a user can (for example) spin up one of our Substrate nodes and send test transactions, which will show how the new functionality works. |
| **0c.** | Testing and Testing Guide | Core functions will be fully covered by comprehensive unit tests to ensure functionality and robustness. In the guide, we will describe how to run these tests. |
| **0d.** | Docker | We will provide a Dockerfile(s) that can be used to test all the functionality delivered with this milestone. |
| 0e. | Article | We will publish an **article**/workshop that explains [...] (what was done/achieved as part of the grant). (Content, language and medium should reflect your target audience described above.) |
| 1. | Substrate module: X | We will create a Substrate module that will... (Please list the functionality that will be implemented for the first milestone. You can refer to details provided in previous sections.) |
| 2. | Substrate module: Y | The Y Substrate module will... |
| 3. | Substrate module: Z | The Z Substrate module will... |
| 4. | Substrate chain | Modules X, Y & Z of our custom chain will interact in such a way... (Please describe the deliverable here as detailed as possible) |
| 5. | Library: ABC | We will deliver a JS library that will implement the functionality described under "ABC Library" |
| 6. | Smart contracts: ... | We will deliver a set of ink! smart contracts that will...


### Milestone 2 Example — Additional features

- **Estimated Duration:** 1 month
- **FTE:**  1,5
- **Costs:** 8,000 USD

...


## Future Plans

Please include here

- how you intend to use, enhance, promote and support your project in the short term, and
- the team's long-term plans and intentions in relation to it.

## Referral Program (optional) :moneybag: 

You can find more information about the program [here](../README.md#moneybag-referral-program).
- **Referrer:** Name of the Polkadot Ambassador or GitHub account of the Web3 Foundation grantee
- **Payment Address:** BTC, Ethereum (USDT/USDC/DAI) or Polkadot/Kusama (aUSD) payment address. Please also specify the currency. (e.g. 0x8920... (DAI))

## Additional Information :heavy_plus_sign:

**How did you hear about the Grants Program?** Web3 Foundation Website / Medium / Twitter / Element / Announcement by another team / personal recommendation / etc.

Here you can also add any additional information that you think is relevant to this application but isn't part of it already, such as:

- Work you have already done.
- If there are any other teams who have already contributed (financially) to the project.
- Previous grants you may have applied for.
