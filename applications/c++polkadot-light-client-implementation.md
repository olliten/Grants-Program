# "Motif": Polkadot Light Client in C++

- **Team Name:** Equilibrium
- **Payment Address:** Ethereum: 0x4126272BA14F7a124b1B98633678e0e5F87Ec478 (USDC)
- **[Level](https://github.com/w3f/Grants-Program/tree/master#level_slider-levels):** 3

## Project Overview :page_facing_up:

This application is for a follow-up grant to [Polkadot Light Client in C++](https://github.com/w3f/Grants-Program/blob/master/applications/c%2B%2Bpolkadot-light-client.md).

### Overview

> A light client or light node is a piece of software that connects to full nodes to interact with the blockchain ... [providing] a much better end-user experience while letting end-users access the blockchain in a decentralized and secure manner.[^1]

[^1]: Source: [parity.io/blog](https://www.parity.io/blog/what-is-a-light-client/)

The goal of this project is to create a new light client implementation for Substrate, written in C++. The client would be an alternative to [substrate-connect](https://wiki.polkadot.network/docs/build-substrate) and other clients like it.

Reasons for having another light client:

- Improve the light client specifications. As for now there is only one [smoldot](https://github.com/paritytech/smoldot)-based light client implementation. This may lead to certain things and workflows being poorly documented, e.g. light client API

Reasons for writing it in C++:
- Involve C++ community into Polkadot development. As for now there is only full node C++ implementation ([Kagome](https://github.com/soramitsu/kagome))
- Add more integration options for mobile and desktop platforms. C++ light client library can be reused for creating mobile and desktop wallets. C++ code of the library will be also compiled to WASM module allowing launching light client in a browser

### Project Details

[![mermaid.png](https://i.postimg.cc/PrgJtfkN/mermaid.png)](https://postimg.cc/56mbgVyM)

Implementation will use a single boost::asio-loop based approach without extra threads. All platform-specific code will be abstracted out. That will allow us to compile the same C++ code both to native binaries and to a WASM module.
- Emscripten will be used to compile C++ code into WASM module.
- [scale-code-cpp](https://github.com/soramitsu/scale-codec-cpp) will be used for SCALE encoding/decoding. 
- [cpp-libp2p](https://github.com/libp2p/cpp-libp2p) will be used as libp2p implementation. We will extend these libraries to support building with the Conan package manager. Also we will add support of building with Emscripten and websocket clients.

To support the light client functionality we will implement the support of the following protocols:

- /dot/sync/warp
- /dot/block-announces/1
- /dot/sync/1
- /dot/light/2

We will use hash_map with serialization/deserialization to hold the blockchain state. In addition, we will use openssl for cryptography functions (e.g. Blake2), and port the needed algorithms not included in openssl (e.g. Schnorr signatures).

#### Requirements

Note: The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in [RFC 2119](https://datatracker.ietf.org/doc/html/rfc2119).

_High-Level Requirements_
- The light client (hereafter _the client_) MUST be written in C++
- The client MUST be compatible with any full nodes of any Substrate chain that are [spec](https://spec.polkadot.network/)-conformant.

_User Experience_
- The client library MUST be architected as to minimize the coupling of functionality and the user interface
  - The client MUST be a command-line application with a support of linux target. The client MUST be implemented in a cross-platform way and SHOULD compile against Mac OS and Windows platforms. But full Windows and Mac OS support is out of the scope and can be added within further development.
  - The client library MUST be usable on popular mobile platforms (iOS, Android)
  - The client library MUST be usable in browsers as a WebAssembly module, with a JS wrapper (hereafter \_WASM Module\_).
  - The client library MUST support linux target. It SHOULD be implemented in a cross-platform way, but full support of other platforms (e.g. Windows, Mac OS, Android) is out of the scope.

- The client MUST be sufficiently documented
  - The client MUST contain standalone documentation for users
  - The client library code SHOULD contain inline documentation for current and future developers

_The WASM Module_
- The WASM Module + JS wrapper SHOULD call RPC methods via API (modelled after ScProvider of [substrate-connect](https://substrate.io/developers/substrate-connect/)), not via RPC server.
- The WASM Module SHOULD NOT concern itself with use in standalone node applications.[^2]

_Functionality_
- The client MUST be able to connecting to a relay chain using chain state.
- The client MUST be able to retrieving checkpoint state from a trusted source to speed up initialization.
- The client MUST be able to subscribe/unsubscribe to/from parachains.
- The client MUST be able to send a new transaction.
- The client MUST be able subscribe a new transaction's updates.
- The client MUST support the following minimum subset of [RPC methods](https://paritytech.github.io/json-rpc-interface-spec/): `rpc_methods`, `chainHead_unstable_follow`, `chainHead_unstable_unfollow`, `chainHead_unstable_unpin`, `chainHead_unstable_storage`, `chainHead_unstable_call` and `chainHead_unstable_stopCall`.

[^2]: This could be discussed in future grant proposals.

## Team :busts_in_silhouette:

#### About Equilibrium

Equilibrium is a software consultancy and venture studio with a mission to further privacy and decentralization. Our clients include StarkWare, ZCash, Aleo, Protocol Labs, The Ethereum Foundation, Polygon, and quite a few more. Our expertise is deeply technical, and we also employ economists and expert cryptographers to deliver results beyond the bounds of software development.	

Our recent work includes:
- [Pathfinder](https://github.com/eqlabs/pathfinder) full node for Starknet
- [Ziggurat](https://github.com/eqlabs/ziggurat) test suite used by several networks

Equilibrium has received three previous grants from the Web3 Foundation, for offchain::ipfs and PolkaFS research, and for the research of the creation of a Polkadot light client in C++ that this proposal is about. 

#### Contact information
- Contact Name: Olli Tiainen
- Contact Email: olli@equilibrium.co
- Website: https://equilibrium.co/

#### Legal structure
- Registered Address: Linnankatu 3, 20100 Turku, Finland
- Registered Legal Entity: Equilibrium Group Oy

#### Team members

- **Vaclav Barta** ([Github](https://github.com/vbar), [LinkedIn](https://www.linkedin.com/in/v%C3%A1clav-b%C3%A1rta-b6842b2/) is a Senior Software Developer at Equilibrium. Having implemented C++ projects for companies from Akamai to Deutsche Bank, currently creating smart contract pipelines from C to WebAssembly for XRPL Foundation.
- **Dmytro Gordon** ([Github](https://github.com/GraDKh), [LinkedIn](https://www.linkedin.com/in/dmytro-gordon-58185b5a/) is a Senior Developer at Eiger (a subsidiary of Equilibrium). He has 10+ years of experience in C++ from various domains including cryptocurrency trading (FineryTech) and portfolio management (Globallogic).
- **Kyle Granger** ([Github](https://github.com/kylegranger) is a Senior Software Developer at Eiger (a subsidiary of Equilibrium). He has 20+ years of experience with C++ in domains including VoIP, WebRTC, block cipher design, broadcast TV, 3D graphics, and industrial cameras.

## Development Status :open_book:

The previous grant for the Polkadot Light Client in C++ focused on extending the Polkadot specification with Light Client requirements and delivering a detailed specification for the implementation of the client outlined in Project Details.
The actual implementation will start conditional to the acceptance of this proposal.

## Development Roadmap :nut_and_bolt:

### Overview

- **Total Estimated Duration:** 4 months
- **Full-Time Equivalent (FTE):**  3 FTE
- **Total Costs:** $359,040

### Milestone 1 — Native light client console application with minimal JSON RPC functionality

- **Estimated duration:** 1 month
- **FTE:**  3
- **Costs:** $89,760

| Number | Deliverable                                   | Specification                                                                                                                                                                                                       |
| ------ | --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0a.    | License                                       | Apache 2.0 / GPLv3 / MIT / Unlicense                                                                                                                                                                                |
| 0b.    | Documentation                                 | We will provide both inline documentation of the code and a basic tutorial that explains how a user can start a light client that connects to one of the chains based on the chain spec and processes RPC requests. |
| 0c.    | Testing Guide                                 | Core functions will be fully covered by unit tests to ensure functionality and robustness. In the guide, we will describe how to run these tests.                                                                   |
| 0d.    | Docker                                        | We will provide a Dockerfile(s) that can be used to test all the functionality delivered with this milestone.                                                                                                       |
| 1      | scale-code-cpp support for Conan              | Add support of Conan package manager to [scale-code-cpp](https://github.com/soramitsu/scale-codec-cpp)                                                                                                              |
| 2      | cpp-libp2p support for Conan                  | Add support of Conan package manager to [cpp-libp2p](https://github.com/libp2p/cpp-libp2p)                                                                                                                          |
| 3      | Transaction management                        | Create a light client application with transaction management functionality.                                                                                                                                        |
| 4      | Chain connection and event processing support | Add support of chainHead\_unstable\_follow, chainHead\_unstable\_unfollow and chainHead\_unstable\_unpin JSON RPC methods                                                                                           |


### Milestone 2 — C++ library with light client functionality

- **Estimated duration:** 1 month
- **FTE:** 3
- **Costs:** $89,760

| Number | Deliverable          | Specification                                                                                                                                                                                        |
| ------ | -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0a.    | License              | Apache 2.0 / GPLv3 / MIT / Unlicense                                                                                                                                                                 |
| 0b.    | Documentation        | We will provide both inline documentation of the code and a basic tutorial that explains how a user can use C++ library in a simple C++ application that connects to chain and can process requests. |
| 0c.    | Testing Guide        | Core functions will be fully covered by unit tests to ensure functionality and robustness. In the guide, we will describe how to run these tests.                                                    |
| 0d.    | Docker               | We will provide a Dockerfile(s) that can be used to test all the functionality delivered with this milestone.                                                                                        |
| 1      | C++ library artifact | Prepare separate C++ library artifact (include + methods)                                                                                                                                            |

### Milestone 3 — JS light client library

- **Estimated duration:** 1 month
- **FTE:** 3
- **Costs:** $89,760

| Number | Deliverable                 | Specification                                                                                                                                                                                      |
| ------ | --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0a.    | License                     | Apache 2.0 / GPLv3 / MIT / Unlicense                                                                                                                                                               |
| 0b.    | Documentation               | We will provide both inline documentation of the code and a basic tutorial that explains how a user can use JS light client library to connect to the chain and process requests in a browser tab. |
| 0c.    | Testing Guide               | Core functions will be fully covered by unit tests to ensure functionality and robustness. In the guide, we will describe how to run these tests.                                                  |
| 0d.    | Docker                      | We will provide a Dockerfile(s) that can be used to test all the functionality delivered with this milestone.                                                                                      |
| 1      | Compilation with Emscripten | Compile cpp-libp2p with Emscripten and add websocket client support                                                                                                                                |
| 2      | WASM compilation target     | Add WASM compilation target for C++ client library                                                                                                                                                 |
| 3      | JS wrapper                  | Implement JS wrapper around WASM module                                                                                                                                                            |

### Milestone 4 — Implement more JSON RPC methods

- **Estimated duration:** 1 month
- **FTE:** 3
- **Costs:** $89,760

| Number | Deliverable                 | Specification                                                                                                                                                                     |
| ------ | --------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0a.    | License                     | Apache 2.0 / GPLv3 / MIT / Unlicense                                                                                                                                              |
| 0b.    | Documentation               | We will provide both inline documentation of the code and a basic tutorial that explains how a user can start a client node/use C++ library and call the implemented RPC methods. |
| 0c.    | Testing Guide               | Core functions will be fully covered by unit tests to ensure functionality and robustness. In the guide, we will describe how to run these tests.                                 |
| 0d.    | Docker                      | We will provide a Dockerfile(s) that can be used to test all the functionality delivered with this milestone.                                                                     |
| 0e.    | Article                     | We will publish an article on Medium that explains why this version of a light client was created, how it was built, and what users can do with it.                               |
| 1      | Additional JSON RPC methods | Add support of _rpc\_methods_, _chainHead\_unstable\_storage_, _chainHead\_unstable\_call_ and _chainHead\_unstable\_stopCall_                                                                           |

## Future Plans

Once the light client is delivered, we hope to continue working on some further performance improvements based on our own ideas during the implementation as well as community feedback. In addition, we’d like to explore the possibility to expand the light client into a full node.

We will also promote the light client on our social media and other marketing channels.

## Additional Information :heavy_plus_sign:

### Why Polkadot Network?

Equilibrium believes the Polkadot (and by proxy Kusama and Substrate) are exceptional for the following reasons:

#### Interoperability
The benefit of using Polkadot for connection is for shared security and interoperability between other Paraobjects or blockchains with XCM, a format for messages that must be passed through (from one Parachain) the Polkadot Relay Chain to another Parachain, as opposed to chains to chains directly. The shared security model and cross Parachains / Relay Chain communications are coupled.
Within Polkadot, XCM allows for interoperability between chains. Bridges from outside the Polkadot ecosystem goes through Parachains or pallet specific research

#### Solid and Consistent Growth
Apart from some common good Parachains supported by stakeholders voting, other Paraobjects must secure slots on Polkadot in order to us it, which requires locking up DOTs, the Polkadot native token. The value of DOTs is therefore highly driven by this demand.
By our estimation, Polkadot is expected to support 50-200 Parachains in the first 2 years of on-boarding parachains (as of Nov 2021). This number is likely higher by now.

#### EVM Compatibility
There are several projects that have EVM compatible pallets and therefore can use the same tooling for development and wallet management. The pallets use may be their own EVM+ by Acala Network or forked version to Frontier by Moonbeam Network and Astra Network.
