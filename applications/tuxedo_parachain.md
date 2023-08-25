# Tuxedo Parachain

- **Team Name:** Off Narrative Labs 
- **Payment Address:** 0x5a335908df9D2C47304338E3b744579Ed7C6a64d (DAI)
- **[Level3](https://github.com/w3f/Grants-Program/tree/master#level_slider-levels):** 3 :chicken:

## Project Overview :page_facing_up:

Allow for Tuxedo runtimes to be parachain runtimes to connect to polkadot  

### Overview

Currently it is only possible to build a Tuxedo runtime for a standalone substrate chain. In order to see more diversity of runtime options for polkadot we would like to allow a UTXO based runtime without frame to enter into the fray. 

### Project Details

We would like to extend the Tuxedo to become a parachain for a few key reasons. One is for Tuxedo runtimes to beable to take advantage of the shared security model of polkadot. This is also allows us to see out a greater vision we have for a Tuxedo runtime to act as an Atomic Swap hub for all DOT ecosystem currencies. Currently Cryp is responsible for delivering a Monero BTC atomic swap protocol [Farcaster](https://github.com/farcaster-project). We have aspirations to extend this facilitate all DOT ecosystem currencies.

The delivery of this Grant would be to achieve a Tuxedo runtime testable on the Rococo testnet which will not initially support XCM(This will require more time to implement this feature in our estimations.) Instead we focus on supporting Cumulus and the absolute basics needed to achieve "parachain" status.

### Ecosystem Fit

Tuxedo is a framework for writing Substrate runtimes. Substrate is the toolkit for building virtually all parachain nodes as well as many standalone blockchains. As such, Tuxedo provides a richer set of options to runtime developers, and hopes to attract teams to the Substrate / Polkadot ecosystem who may have otherwise gone elsewhere.

The primary users of Tuxedo will be parachain and runtime developers who will use Tuxedo directly to structure their runtimes. Of course, the user base will trickle downstream as well to users of those parachains that choose to build with Tuxedo. However, chain users will use Tuxedo only indirectly.

There are no projects like this in the Substrate ecosystem, although they do exist in the broader blockchain space; Cardano being the most notable example.

While it fulfills a similar role, Tuxedo is not intended to compete with FRAME, but rather to compliment it, by welcoming projects that fit naturally with the utxo model into the Substrate ecosystem, as FRAME does for projects that fit the accounts model.

## Team :busts_in_silhouette:

### Team members

- Joshy Orndorff https://github.com/JoshOrndorff
- Off Narrative Labs https://github.com/Off-Narrative-Labs

### Contact

- **Contact Name:** Joshy Orndorff 
- **Contact Email:** joshyorndorff at proton dot me
- **Registered Address:** To be provided in the invoices

### Team's experience

Joshy entered the Substrate ecosystem in 2019 as part of the Substrate Developer Hub team. There he created and hosted the weekly Substrate Seminar, and contributed significantly to the Substrate Recipes. In 2020, he moved to the Moonbeam team where he was a core developer. While at Moonbeam, Joshy wrote the [Nimbus consensus engine](https://github.com/PureStake/nimbus/) which is used in several production parachains, and helped pioneer the technique whereby EVM contracts can interact with native Substrate pallets. In 2022, Joshy began contributing to the Polkadot Blockchain Academy, teaching in all three cohorts: Cambridge, Buenos Aires, and Berkeley.

Joshy met Andrew Burger in Cambridge in 2022 at the first Polkadot Blockchain Academy where Andrew, a student at the time, chose to implement a UTXO Runtime as his final project. Together they revised the UTXO assignment and taught it together at the next PBA. A few months later they worked together on the first Tuxedo grant.

Andrew will not be contributing to this grant work (although he is still an enthusiastic open source contributor to Tuxedo) due to employment elsewhere.

## Development Status :open_book:

The team has done previous work on this topic:
[tuxedo](https://github.com/Off-Narrative-Labs/Tuxedo/tree/main)

Some lingering leftovers on Tuxedo is mostly centered around the wallet. We do at some point want to make the wallet more generalizable to beable to craft any generic transaction based on whichever tuxedo runtime is defined. Currently, it is somewhat bound to the money usecase. This however does not affect our ability to push forward in our overall vision. 

## Development Roadmap :nut_and_bolt:

### Overview

- **Total Estimated Duration:** 9 weeks
## TODO Is this accurate regarding FTE?
- **Full-Time Equivalent (FTE):**  1.5 FTE 
- **Total Costs:** $72,000 (USD)

### Milestone 1 — Convert Pallet Parachain System to Tuxedo Piece 

- **Estimated duration:** 4 weeks
- **FTE:**  1.5
- **Costs:** 32,000 USD

One of the main things that makes a Substrate runtime a parachain runtime is the `cumulus_pallet_parachain_system` 

| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| **0a.** | License | Apache 2.0 |
| **0b.** | Documentation | We will provide both **inline documentation** of the code and a basic **tutorial** that explains how a how the parachain system piece works within Tuxedo as compared to FRAME |
| **0c.** | Testing and Testing Guide | Core functions will be fully covered by comprehensive unit tests to ensure functionality and robustness. |

### Milestone 2 — Migration of `register_validate_block!` 

- **Estimated Duration:** 2 weeks
- **FTE:**  1.5
- **Costs:** 20,000 USD

Another requirement for a parachain runtime is the `register_validate_block!` macro

We will need to migrate the following to a Tuxedo compatible version

```rust
cumulus_pallet_parachain_system::register_validate_block! {
	Runtime = Runtime,
	BlockExecutor = cumulus_pallet_aura_ext::BlockExecutor::<Runtime, Executive>,
	CheckInherents = CheckInherents,
}
```

| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| **0a.** | License | Apache 2.0 |
| **0b.** | Documentation | We will provide both **inline documentation** of the code and a basic **tutorial** that explains the difference between the FRAME based approach and the Tuxedo based approach to this macro |
| **0c.** | Testing and Testing Guide | Core functions will be fully covered by comprehensive unit tests to ensure functionality and robustness. In the guide, we will describe how to run these tests. |

### Milestone 3 — Full Testing via Rococo 

- **Estimated Duration:** 2 weeks
- **FTE:**  1.5
- **Costs:** 20,000 USD

Fully test the setup on both local and public Rococo relay chains

| Number | Deliverable | Specification |
| -----: | ----------- | ------------- |
| **0a.** | License | Apache 2.0 |
| **0b.** | Documentation | We will provide both **inline documentation** of the code and a basic **tutorial** that explains how a user can spin up the example node and transfer tokens |
| **0c.** | Docker | We will provide a Dockerfile that can be used to test all the functionality delivered with this milestone. |


## TODO Are the milestones descriptive enough anything that anyone would add?

## Future Plans

We have a few key future plans which include

    1. XCM integration with Tuxedo for Cross-chain UTXOS
    2. Atomic Swaps with Tuxedo from various UTXO chains such as BTC, Monero, ZCash etc.
    3. DOT ecosystem Atomic swaps with various UTXO chains

## Additional Information :heavy_plus_sign:

The team has been in the Substrate ecosystem for a long time, so we have heard of the grants program in many ways. From colleagues, grant recipients speaking highly the program, and grant recipients looking for help understanding Substrate.
