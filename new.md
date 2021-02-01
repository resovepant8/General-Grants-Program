# Raze Network - Open Grants Program

* **Project:** Raze Network
* **Proposer:** [Raze Network](https://www.raze.network/)
* **Payment Address:** 3QozZhdPC4ecxK3y8iGCG6WN4wH6KvhsEy

## Project Overview :page_facing_up:

### Overview

#### Introduction

Raze Network is a second-layer protocol that will provide cross-chain payment privacy for the entire DeFi stack of the Polkadot ecosystem. The core technical module of the Raze Network is a second-layer decentralized anonymous payment module, which will serve as a universal plugin-and-play infrastructure for the Polkadot DeFi ecosystem. This module will be imported as a substrate-based smart contract, which allows the user to hide one's account address and financial information (as shown in the Figure below) before participating the Polkadot DeFi stacks, be it DEX, liquidity mining, loan, insurance, etc. Since the Zether framework was designed to support the Ethereum account model, our contract will support both the mainstream token issued in the Polkadot ecosystem such as DOT/KUSAMA and ERC-20 cross-chain private payment, and hence significantly increase the liquidity of the Polkadot ecosystem. 

The objective of Raze Network is to enable cross-chain privacy-preserving payment and DeFi systems, while protecting the transparency of your assets and behaviors from surveillance. Eventually, Raze Network can anonymize all the cryptocurrency in the world.
<p align="center">
  <img src="https://raw.githubusercontent.com/huaihuaisz/General-Grants-Program/master/src/image2.png" alt="" width="70%"/>
</p>

#### Team Interest

The Raze Network team has deep knowledge of zero-knowledge algorithms and applied zkSNARK techniques to Ethereum platform. Our team also has lots of experience in designing and implementing trustless zksnark schemes. The background of the team members consists of both engineering and academic talents.

Substrate is a new and promising platform to implement the privacy protocol and leverage the cross-chain features to achieve state-of-the-art privacy solutions to all the DeFi applications. Thus, we are confident that we have the capability to bring more privacy contributions to the Substrate framework as well.

### Project Details
#### Project Architecture

We will apply the Zether framework to build the second-layer decentralized anonymous payment module. It will be then imported as substrate-based smart contracts. Similar to the Zether framework, it will have three technical modules: mint, transfer and redeem. The mint module will convert a token into its anonymized version, while the redeem module will convert the anonymized token back to its native form. The transfer module is the one that enables the anonymous transfer of the anonymized token. It will conceal the transaction amount and guarantee  anonymity for both sender and receiver.
<p align="center">
  <img src="https://raw.githubusercontent.com/huaihuaisz/General-Grants-Program/master/src/image5.png" alt="" width="70%"/>
</p>

The mint module will create a Raze account by running a mint contract and deposit the anonymized token to the Raze account. Each Raze account is identified by a public key `pk` and the mint module will generate a ciphertext under the account public key which encrypts the amount of minted token. If the account has already existed before the mint operation, the generated ciphertext will be homomorphically added to the existing ciphertext to increase the encrypted amount.

Since the balance and transaction amount of each Raze account is encrypted and therefore hidden, the remaining question for the transfer module is how to hide the identities of the involved parties. Similar to the anonymous transfer module of the Zether scheme, we hide the identities of the involved parties through the "one-out-of-many" proof. The "one-out-of-many" proof is conceptually close to ring signature, which proves that a transaction is launched by one of the many parties in the anonymity set (or the ring) while not revealing exactly whom among them is the sender or receiver of the transaction, and thus hide their identities. The transfer module's zkp will also prove the payment consistency and provide a range proof demonstrating there is no negative amount involved in the transaction, which could potentially allow the adversary to create money out of thin air.

The redeem module converts the anonymized token back to its original form. The redeem module also needs to invoke a zero-knowledge proof functionality to prove that the user initiating the redeem module knows the secret key of the corresponding Raze account and the redeemed amount is smaller than the Raze account balance.

To sum up, we use public-key homomorphic encryption to ensure the balance and transactional amount confidentiality and invoke the "one-out-of-many" proof to hide the sender and receiver identities, and some other zero-knowledge proof schemes to guarantee the payment consistency. The Raze Network can be viewed as a pool of boiling water, where each water molecule interacts with each other in a chaotic and vibrant fashion. Whenever a user deposits a certain amount of token through invoking the mint module, the token would be like a water molecule dropping into this pool of boiling water, it is no longer traceable.

To facilitate the DeFi functionality, we have two additional modules: lock and unlock. The lock module would allow an account owner to lock the account while the unlock module allows the account owner to unlock the account.

We will also build a cross-chain bridge that can map any ERC-20 token to the Polkadot blockchain and thus enable the cross-chain payment of these tokens.
<p align="center">
  <img src="https://raw.githubusercontent.com/huaihuaisz/General-Grants-Program/master/src/image1.png" alt="" width="70%"/>
</p>

Each raze user can register a raze account any time(s) he wishes. The registration algorithm CreateAddress generates a secret key `sk` and the corresponding public key `pk`. The public key is the identifier of the raze account.

To invoke the Mint contract, the client-side runs a CreateMintTx algorithm, which takes as input the raze account `pk` and the amount of native token `amt` as inputs. The output of the CreateMintTx algorithm is a ciphertext `cp_1` encrypted under the public key `pk` If the raze account `pk` is already attached with a ciphertext `cp_0`, the newly created ciphertext will be homomorphically added to the existing ciphertext to increase the encrypted amount. The updated ciphertext will be formed as `cp_1*cp_0`. Otherwise, the new ciphertext will be attached to the raze account. The native token will be stored in the mint contract.

To invoke the transfer contract, the client-side runs a CreateTransferTx algorithm, which takes as inputs the raze account secret key `sk`, and the amount of transferred token `amt`, the public keys of sender `pk_s`, receiver `pk_r` and the public keys of the anonymity set `{pk_a}`. The output of the CreateTransferTx algorithm is a zero-knowledge proof that the prover knows one of the secret keys of the aforementioned public key set, the payment consistency proof, and range proof. The statement of this zkp is 
<p align="center">
  <img src="https://raw.githubusercontent.com/huaihuaisz/General-Grants-Program/master/src/image3.png" alt="" width="60%"/>
</p>

The client-side runs a CreateRedeemTx algorithm to invoke the redeem contract. It takes the account secret key `sk`, the withdrawal amount `amt`, and the public key `pk` as input to generate a zero-knowledge proof showing that the user knows the secret key `sk` for the account public key `pk` and the account has enough balance for the redeem operation. The zero-knowledge proof will be used to invoke the redeem contract. The statement of this zkp is 
<p align="center">
  <img src="https://raw.githubusercontent.com/huaihuaisz/General-Grants-Program/master/src/image4.png" alt="" width="40%"/>
</p>
 
The user can invoke the lock module by running a CreateLockTx algorithm on the client-side. The client inputs a secret key `sk` and an Ethereum address `addr` to generate a signature to demonstrate he is indeed the owner of the account and he authorizes to lock the account to the input address `addr`. The signature would be `Sign(x, addr)`. Similarly, the user can invoke the unlock module by running a CreateUnlockTx on the client-side. The input of CreateUnlockTx algorithm is the same as that of the CreateLockTx algorithm. It will generate a similar signature to unlock the account. Note, we will embed a nonce derived from the current epoch number to prevent the replay attack.

##### Substrate/Polkadot Integration

The Raze Network will be implemented as Substrate modules. More specifically, we will build a raze substrate pallet that supports:
* The user could mint an anonymized token by invoking the Mint module. It will support all the mainstream tokens issued on the Polkadot blockchain such as DOT or KUSAMA, and ERC-20 tokens.
* A user who owns a raze account could transfer the anonymized token to another raze account by running the Transfer module to hide the identities of the involved parties and the transferred amount.
* A user could run the Redeem module to convert the anonymized token back to its native form.
* The lock and unlock modules will allow the users to participate in the anonymity mining, and thus provide incentives to the users to join the Raze Network and thus increase the anonymity set.

The ledger state will mainly keep a record of the raze accounts, the balance encryption, and the pending transfer tables, etc.

##### RazeVM and SDK

Our ultimate goal is to provide a SDK from a high-level perspective with the above functions. The RazeVM will include the following modules described above.The SDK will include the client that supports the **Register**, **CreateMintTx**, **CreateTransferTx**, **CreateRedeemTx**, **CreateLockTx**, and **CreateUnlockTx** algorithms. The clients will be able to generate the necessary transactions to trigger the corresponding modules.

#### Ecosystem Fits

Decentralized finance (DeFi) in the Polkadot ecosystem is still a newborn. Privacy is essential to the success and prosperity of DeFi. The Raze Network will serve as a vital infrastructure for the future development of privacy-preserving DeFi in the Polkadot ecosystem. The Raze Network will not only liberate the Polkadot DeFi ecosystem from the surveillance of the big brother, but also significantly increase the liquidity of the Polkadot ecosystem. The closest product to the Raze Network is the Manta Network, which is a privacy-preserving DEX system. Our proposal differs from theirs in the following regards:
* The Zether framework is designed for the Ethereum account-based model, and hence it naturally facilitates the cross-chain private payment of ERC-20 tokens in the Polkadot ecosystem. Since all the aforementioned smart contract modules are supposed to be imported as Substrate-based modules, they will also be compatible with the tokens issued in the Polkadot ecosystem, such as DOT or KUSAMA. To sum up, our work will significantly increase the liquidity of the Polkadot system due to its cross-chain interoperability.
* Our product is much more generic in the sense that it supports all the DeFi products in the Polkadot system, and it also supports anonymity mining, which in itself is already a vital DeFi feature. In contrast, the Manta Network focuses solely on developing the private DEX system, which limits the scope of its application scenarios severely.
* From the technical perspective, the underlying zero-knowledge proof scheme for the Manta Network requires trusted setup, which is why they require a trust ceremony. In contrast, the zero-knowledge proof scheme we adopt does not require any trusted setup. All the public parameters of our system can be randomly sampled from the underlying group in a totally transparent fashion, which is much more in accordance with the decentralization ethos.

#### The Purpose of this Grant

The purpose of this grant is to deliver Raze Substrate Modules and cross-chain bridge implementation. The main deliverable of this grant is Raze substrate pallet that supports: mint, transfer, redeem, lock and unlock functionalities. The substrate modules will support both the mainstream tokens issued in the Polkadot ecosystem such as DOT and KUSAMA and the cross-chain payment of ERC-20 tokens.

## Team :busts_in_silhouette:
### Team Members

* Ryuhei Matsuda - Blockchain Specialist
* Jark Ling - Full-stack Developer
* Micheal Andersen - Full-stack Developer
* Dr.Hou - Cryptography Researcher and Academic Advisor

### Team Website

* [www.raze.network](http://www.raze.network)

### Legal Structure
* Digital Asset Investment Management Ltd. is a company registered in Cayman Islands.

### Team Experience

#### Ryuhei Matsuda

Ryuhei is adept at javascript, go, java, c++, and Rust, Ryuhei has proven his ability over the years to help architects and lead large projects or join existing teams with equal ease. Ryuhei was previously CTO at Trade Pharma Network and Lead Developer at BlocX. He graduated from the University of Tokyo.

#### Jark Ling

Jark is an expert in the architecture and design of Blockchain applications and Substrate pallets. He has 8 years of experience in software development and he is the community contributor in Linux.

#### Micheal Andersen

Micheal is a full-stack developer with 6 years experience in software development and technology management. He has plenty of experience in Software Architecture and currently focuses on blockchain development and cross-chain technologies.

### Team Code Repos
* https://github.com/Raze-Net

## Development Roadmap :nut_and_bolt:
### Overview

* Total Estimated Duration: 3 months
* Full-time equivalent (FTE): 3
* Total Costs: 1.5 BTC

#### Milestone 1 — Raze Substrate Modules and cross-chain bridge implementation

| **Number** | **Deliverable**                          | **Specification**                                            |
| ---------- | ---------------------------------------- | ------------------------------------------------------------ |
| 0a.        | License                                  | Apache 2.0 / MIT / Unlicense                                 |
| 1.         | Raze Substrate module for private payment  | We will implement the zero-knowledge proof schemes and create a Substrate module that incorporates the verification logic for the aforementioned modules. It will support the verification of mint, transfer, redeem, lock and unlock. |
| 2.         | A cross-chain bridge between Ethereum and Polkadot | The bridge will map ERC-20 tokens to the Polkadot ecosystem and facilitate their private payment in the Polkadot ecosystem. |
| 3.         | Benchmark | Benchmark on the throughput and gas cost of the proposed modules. |
| 4.         | Docker    | We will provide a dockerfile to demonstrate the usage of our modules. |

#### Milestone 2 — Raze client implementation and integration

* Estimated Duration: 1 month
* FTE: 1
* Costs: 0.5 BTC

The main deliverable of this milestone is the client that can generate the transactions that can trigger the aforementioned contracts.

| **Number** | **Deliverable**                          | **Specification**                                            |
| ---------- | ---------------------------------------- | ------------------------------------------------------------ |
| 0a.        | License                                  | Apache 2.0 / MIT / Unlicense                                 |
| 1.         | Raze client | We will implement the client that supports the Register, CreateMintTx, CreateTransferTx, CreateRedeemTx, CreateLockTx, and CreateUnlockTx algorithms. The client will be able to generate the necessary transactions to trigger the corresponding substrate modules. |
| 2.         | Anonymity mining | Through combining the lock and mint, and unlock and redeem modules, we will implement anonymity mining functionality, which allows the users to mine the private tokens and unlock the private tokens after a certain period of time. |
| 3.         | Benchmark | Benchmark on the usability and latency of the proposed client functionalities. |
| 4.         | Docker    | We will deploy the client on Kusama or Rococo and engage our community on the testing of our product. |

### Community Engagement

* **DeFi Developers:** Raze Network focuses on giving developer support through both offline and online channels. Developers can integrate the RazeVM with only a few lines of code. The simplicity of integrating this project makes it easy for any developer to experiment.
* **Anonymity Mining:** By using Raze Network, users also mine RAZE, the governance token of Raze Network. The more users use it, the more say you have in the evolution of the protocol.

## Future Plans

In phase 1, our goal is to achieve all the basic functions of Raze Substrate Modules and cross-chain bridge implementation.

In phase 2, we will open the source code of the mint module and the redeem module and launch the product based on the source code of mint and redeem module and it can be accessible by metamask or other decentralized walle.

In phase 3, we will launch the product based on the source code of cross-chain anonymous payment as a substrate pallet.

Finally, our goal is to provide a SDK from a high-level perspective with the above tools as RazeVM.

## Additional Information :heavy_plus_sign:

