# Blockchain Mechanics and Digital Ledger Technology

**Study Guide**

FIN-4600 Fintech Foundations  |  Michigan Technological University

Companion to the lecture deck and to Nofsinger, Stein Smith & Bonaparte, Survey of Fintech (2026), Chapters 2 and 3.

*Big picture: by the end of this guide you should be able to explain how a blockchain keeps a shared record without a central record keeper, how mining and consensus protect it, and where the technology does and does not add value in finance.*

## Learning Objectives

- **LO 1 —** Describe a distributed ledger and explain how hashes, blocks, Merkle roots and mining make records tamper-evident. (Nofsinger LO 2.1–2.3)
- **LO 2 —** Compare proof of work, proof of stake, proof of authority and BFT consensus. (Nofsinger LO 2.4–2.5)
- **LO 3 —** Describe wallets, custody and exchanges, and explain what FTX showed about controls. (Nofsinger LO 3.1–3.2)
- **LO 4 —** Distinguish blockchain types and evaluate financial and non-financial applications. (Nofsinger LO 3.3–3.4)
- **LO 5 —** Analyze the DTCC tokenization project and the SEC and Nasdaq approvals.
- **LO 6 —** Explain why ICO fraud occurred and how the SEC responded.
- **LO 7 —** Compare Swift's ledger, Ripple and stablecoins for cross-border payments, and what separates successful projects from failures.

## Tools for This Guide

- **Lecture deck and script:** slide numbers appear next to each section below; a narration script is provided separately.
- **Python:** the hashing exercise at the end uses only the standard library.
- **Live data:** look up any Bitcoin block on a block explorer such as mempool.space.
- **Demo:** Brownworth's interactive blockchain demo lets you mine a block in the browser.

### Where this guide meets the textbook

| Topic | Textbook location | Slides |
|---|---|---|
| Ledgers, history, Bitcoin origins | Background to Ch. 2 §2.1 | 4–8 |
| Decentralized and distributed; immutability; transparency | Ch. 2 §2.1, Figure 2.1 | 10 |
| Bitcoin blockchain, nodes, hashes, nonce, difficulty | Ch. 2 §2.2 | 11–13 |
| Mining, block reward, halving, fees; block 843,909 | Ch. 2 §2.2.1–2.2.3, Figures 2.2–2.4 | 14–17 |
| Consensus mechanisms and hybrids | Ch. 2 §2.3, Table 2.1 | 18–25 |
| Wallets, custodians, exchanges, FTX | Ch. 3 §3.1–3.2, Fintech in the World | 26–29 |
| Types of blockchains; smart contracts; non-financial uses; accounting | Ch. 3 §3.3–3.5, Table 3.1 | 30–33 |
| Custody rules (SEC proposal) and OCC trust charters | Extends Ch. 3 §3.1.2 (custodians); see supp-01 and ce-01 | 34–35 |
| Tokenization and the DTCC case | Extends Ch. 3 §3.3.3 (financial services) | 36–41 |
| ICO fraud and SEC enforcement | Background to Ch. 3 §3.2 (exchanges, assets) | 42–46 |
| Cross-border payments: Swift, Ripple, stablecoins | Extends Ch. 3 §3.3.3; stablecoins | 47–53 |
| Successes, failures, limits, digital money | Ch. 3 §3.3–3.4 plus current cases | 54–65 |

Chapter 3 (Blockchain Applications) covers wallets, exchanges, blockchain types, enterprise uses and accounting. Parts 5 to 8 of the lecture go beyond the textbook with current cases.

## 1  Ledgers and History  (Slides 4–8)  — LO 1

- **Digital ledger:** an electronic record of transactions and asset ownership. A traditional ledger has one keeper; a distributed ledger has many nodes holding identical copies.
- **Why it matters in finance:** less reconciliation, shared records and tamper evidence, in exchange for complexity and limited throughput.
- **Building blocks before Bitcoin:** public-key cryptography (Diffie and Hellman, 1976); anonymous digital cash (Chaum, 1982); chained timestamped records (Haber and Stornetta, 1991); Hashcash proof-of-work stamps (1997); b-money and bit gold (1998); reusable proof of work (Finney, 2004).
- **Bitcoin (2008–2009):** the whitepaper solves double spending without a trusted third party by combining proof of work, a longest-chain rule and mining rewards. The genesis block (Jan. 3, 2009) embeds a newspaper headline about bank bailouts.

## 2  Blocks, Hashes and Mining  (Slides 9–17)  — LO 2, LO 3

### 2.1 Decentralized and distributed (Nofsinger Ch. 2 §2.1)

- **Every node keeps a complete copy** of the blockchain, so there is no single point of control or failure (Figure 2.1).
- **Immutable:** cryptography makes past records extremely hard to change without detection.
- **Transparent:** anyone can inspect the public ledger with a block explorer.

### 2.2 Inside a Bitcoin block (Nofsinger Ch. 2 §2.2)

- **Block header:** version, previous block hash, Merkle root, timestamp, difficulty target and nonce.
- **Hash:** SHA-256 produces a 256-bit fingerprint. Change one character of the input and the output looks unrelated.
- **Merkle root:** one hash that summarizes every transaction in this block. The previous block hash is what links the chain.
- **Nonce:** a 32-bit number miners vary by trial and error until the block hash falls below the target.
- **Why tampering fails:** altering one old transaction changes its Merkle root and block hash, which breaks every later link. An attacker must redo the proof of work for that block and all that follow.

### 2.3 Mining, rewards and fees (Nofsinger Ch. 2 §2.2.1–2.2.3)

- **Proof of work:** miners compete to find a valid nonce. Difficulty retargets every 2,016 blocks to keep about 10 minutes per block.
- **Block reward:** halves every 210,000 blocks (50, 25, 12.5, 6.25, 3.125 BTC; the next halving is expected around 2028). Supply is capped near 21 million.
- **Validators** check transaction formats, verify block details and accept or reject new blocks. Fees rise when demand for block space is high.
- **Worked example:** block 843,909 (May 17, 2024, mined by SBI Crypto) held 3,334 transactions and 2,866.67 BTC. The miner earned 3.125 BTC plus 0.1076 BTC in fees.
- **Capacity note:** the historic 1 MB limit became a 4,000,000 weight-unit limit after SegWit, which is why block 843,909 was about 1.48 MB.
- **Throughput:** Bitcoin's base layer handles a handful of transactions per second; Ethereum's base layer roughly 15 to 30. Visa filings cite capacity above 65,000 messages per second (a design figure, not average load).

## 3  Consensus Mechanisms  (Slides 18–25)  — LO 4

A consensus mechanism lets nodes that do not know or trust each other agree on one ledger state. Think of it as a cost or stake that stops fake identities, plus a rule for choosing the valid chain (Nofsinger Ch. 2 §2.3).

| Mechanism | Key process | Strength | Weakness | Examples |
|---|---|---|---|---|
| **Proof of work** | Miners solve puzzles | Security, decentralization | Energy, slow | Bitcoin, Litecoin |
| **Proof of stake** | Validators chosen by stake | Efficient, fast | Wealth concentration | Ethereum (since 2022), Cardano |
| **Delegated PoS** | Holders vote for delegates | High throughput | Collusion, low turnout | EOS, TRON |
| **Proof of authority** | Approved authorities take turns | Very fast, cheap | Centralized trust | Private networks |
| **BFT** | Nodes agree despite bad nodes | Resilient | Hard to scale | Hyperledger Fabric v3 (SmartBFT) |
| **Hybrids** | PoW/PoS; PoS/BFT | Blend strengths | Complexity | Hcash; Solana (Tower BFT) |
| **Emerging** | Burn, elapsed time, capacity | Low energy | Niche risks | Slimcoin; Sawtooth; Burstcoin |

- **Update to note:** Hyperledger Fabric has used Raft (crash fault tolerant) since v1.4. Version 3.0 added a Byzantine fault tolerant ordering service (SmartBFT).
- **Ethereum** moved from proof of work to proof of stake in September 2022 (“the Merge”).
- **Choosing:** ask who may validate, how many nodes, how fast payments must be final, and what energy and cost the network can bear.

## 4  Wallets, Exchanges, Enterprise  (Slides 26–35)  — LO 3–4

### 4.1 Wallets and custody (Nofsinger Ch. 3 §3.1)

- **A wallet stores keys, not coins.** The public key works as an address; the private key proves ownership. Lose the private key and the asset is lost.
- **Wallet types:** hot (online, convenient), cold (offline hardware), multisig (several keys must act), smart wallets (run contract code).
- **Custodians** safekeep assets, process trades and service holdings. Crypto custodians need insurance and cyber protection; hot-wallet custody adds third-party risk. Proof-of-reserves audits check that on-chain holdings match client claims.

### 4.2 Exchanges and the FTX collapse (§3.2)

- **Centralized exchanges** have an operator, easy bank linking and liquidity, but hold your keys. Decentralized exchanges are peer to peer with no oversight and need technical skill.
- **FTX:** a CoinDesk report on Nov. 2, 2022 showed FTX and Alameda Research funds were blurred. After Binance's CEO said he would sell FTT, about $5 billion was withdrawn on Nov. 6. FTX went bankrupt, about $600 million was reported drained, and BlockFi, which had lent $275 million, also failed.
- **Lessons:** weak controls, risk management and operations; commingling; audit firms leaving crypto when transparency was most needed.

### 4.3 Blockchain types and enterprise uses (§3.3–3.5)

| Type | Key feature | Strength | Weakness |
|---|---|---|---|
| **Public** | Open to anyone | Transparent, trustless | Energy, slower |
| **Private** | One controlling entity | Fast, controlled | Centralized |
| **Hybrid** | Mixes public and private | Customizable privacy | Complex |
| **Consortium** | Run by a group of firms | Efficient for members | Governance risk |
| **Sidechain** | Linked to a parent chain | Adds scalability | Security risk |

- **Smart contracts:** terms in code that execute automatically (for example, insurance payouts). Immutability makes bugs costly, so audit before deployment.
- **Triple-entry accounting:** debit and credit plus a shared cryptographic receipt.
- **Non-financial uses (§3.4):** digital vehicle titles (California DMV), Walmart food traceability (days to seconds), credentialing, medical records, Internet of Things micropayments. Check the current status of named projects.
- **Accounting and controls (§3.5):** custody and access controls matter more; immutable data raises import and export controls; systems must interoperate with non-blockchain technology; tokens can carry reporting effects.

### 4.4 Custody rules and trust charters (current events, slides 34–35)

- **SEC proposal (Oct. 1, 2026):** would let advisers and regulated funds self-custody crypto assets under conditions, and add state trust companies as permitted custodians. A proposal only; comments are open for 60 days after Federal Register publication. See supp-01.
- **OCC trust charters:** the OCC conditionally approved five crypto-related national trust banks on Dec. 12, 2025, approved Protego's charter conditionally in Feb. 2026, and finalized a rule (effective Apr. 1, 2026) saying trust banks are not limited to fiduciary activities. ICBA sued the OCC on Oct. 2, 2026; no court has ruled. See ce-01.

## 5  Tokenizing Securities: the DTCC Case  (Slides 36–41)  — LO 5

Tokenization means a token on a ledger stands for a claim on an asset. The case below shows how it is being introduced inside regulated US market infrastructure.

| Date | Event |
|---|---|
| **Dec. 11, 2025** | SEC staff no-action letter: DTC may offer a defined tokenization service for three years |
| **Dec. 17, 2025** | DTCC, Digital Asset and the Canton Network announce plans to mint DTC-held Treasuries |
| **Mar. 18, 2026** | SEC approves Nasdaq rule SR-NASDAQ-2025-072 to trade tokenized securities |
| **May 4, 2026** | DTCC convenes 50+ firms; plans July trials and an October launch |
| **Jul. 15, 2026** | First live production trades with about 40 firms |
| **Oct. 2026** | Full DTCC Tokenization Service scheduled to launch (verify status) |

- **What is tokenized:** an on-chain “digital twin” (tokenized entitlement) of a security held at DTC, with the same ownership, dividend and voting rights, convertible back.
- **Where it runs:** Canton (a privacy-enabled network built for regulated finance) and Hyperledger Besu.
- **Scope:** Russell 1000 stocks, major index ETFs and US Treasuries.
- **What does not change:** the trade still settles T+1; tokens are created after settlement; Nasdaq order books, surveillance and priority rules are unchanged; securities-law duties still apply. The master securityholder record stays at DTC.
- **Limits in the base version:** per the no-action letter, DTC gives tokens no special collateral or settlement value; digital cash settlement is planned for 2027.
- **Why it matters:** tokens can move instantly after creation (for example, as margin collateral), and the approach sits inside existing regulation. Commentators call the Nasdaq approval a regulatory milestone, not an operational revolution.
- **Contrast:** the SEC's January 2026 staff statement separates tokens made by or for the issuer from those made by unaffiliated third parties; the two paths carry different rights.

## 6  ICO Fraud and the SEC  (Slides 42–46)  — LO 6

- **What an ICO was:** a project sells new tokens for Bitcoin or Ether, with a white paper instead of a prospectus and no audit or disclosure regime.
- **The DAO (2016):** an Ethereum fund; a hacker drained about 3.7 million ether (about 30 percent of its funds) and Ethereum forked to undo it. The SEC's July 2017 DAO Report said such tokens can be securities under the Howey test (investment of money, common enterprise, profits from others' efforts).
- **How much was fraud:** Satis Group (2018) found 78 percent of 2017 ICOs, by count, were identified scams, but only about $1.3 billion of $11.9 billion raised (about 11 percent) went to them, mostly two projects (Pincoin about $660 million, Arisebank about $600 million). Another 4 percent failed, 3 percent went dead and 15 percent traded. This is an industry report, not an audit.

| When | Action |
|---|---|
| **Jul. 2017** | SEC DAO Report: token sales can be securities offerings |
| **Sep. 2017** | First SEC ICO fraud case: REcoin and a diamond-backed ICO |
| **Dec. 2017** | Munchee: a so-called utility token treated as a security because of how it was marketed |
| **Nov. 2018** | First civil penalties for unregistered ICOs (Airfox, Paragon) |
| **Sep. 2019** | Block.one pays $24 million for its unregistered ICO |
| **Jun. 2020** | Telegram returns about $1.2 billion to investors and pays an $18.5 million penalty |

- **Also:** CFTC actions against My Big Coin and CabbageTech (Jan. 2018); the SEC sued Kik (2019); celebrity promoters of Centra Tech were sanctioned.
- **Red flags:** guaranteed returns, celebrity promotion, anonymous team, white paper only, no working product, pressure to buy now.
- **Lessons:** substance beats the “utility” label; enforcement pushed the market toward registered and exempt offerings; later clarity helped enable regulated paths such as the DTCC and Nasdaq rules.

## 7  Cross-Border Payments  (Slides 47–53)  — LO 7

- **The problem:** correspondent-bank chains add fees, checks and delay; cut-off times and pre-funded accounts trap liquidity. Swift reports 75 percent of its payments reach the beneficiary bank within 10 minutes, so cost, transparency and 24/7 availability matter too. From November 2026 Swift requires structured ISO 20022 address data in cross-border messages.
- **Swift shared ledger:** announced Sept. 2025, live pilot July 9, 2026 with 17 banks across six continents (including BNY, Citi, HSBC, UBS). Banks issue tokenized deposits on their own ledgers; the shared ledger orders their commitments around the clock; final settlement initially uses existing systems. Built with Consensys on an Ethereum-compatible permissioned stack (sources differ on the exact network). It has no native coin and complements Swift messaging.
- **Ripple:** Ripple Payments links banks over the XRP Ledger; XRP can act as a bridge currency; RLUSD stablecoin launched 2024. In the SEC case, a 2023 ruling held programmatic XRP sales were not securities but institutional sales were; a $125 million penalty and an injunction stand after appeals were dropped in Aug. 2025. On Dec. 12, 2025 the OCC conditionally approved Ripple National Trust Bank (conditional, not final); RLUSD continues to be issued under Ripple's New York trust charter. Ripple's $70+ billion annual volume is a company figure.
- **Stablecoins:** about $308 billion outstanding in Aug. 2026, nearly all dollar-backed; Tether about 60 percent, USDC about 24 percent. Visa reported a stablecoin settlement run-rate near $4.5 billion (Jan. 2026). The GENIUS Act (July 2025) requires 1:1 reserves and bars interest; final rules were due in 2026 with enforcement from 2027. Risks: de-pegging, money laundering, reserve quality. Raw on-chain volume overstates payments.
- **JPM Coin:** from a private Quorum chain (2019) to the JPMD deposit token on the public Base network (2025); a deposit token is a claim on a bank deposit, not a stablecoin.
- **Wholesale CBDCs:** cross-border projects such as mBridge use central bank money; e-CNY accounts for more than 95 percent of its volume.

| Route | Money form | Run by | Status, 2026 |
|---|---|---|---|
| **Swift ledger** | Bank tokenized deposits | Bank cooperative | Live pilot, 17 banks |
| **Ripple** | XRP, RLUSD | Private company | Operating, bank pilots |
| **Stablecoins** | USDT, USDC and others | Private issuers | $300B+; GENIUS rules |
| **Wholesale CBDC** | Central bank money | Central banks | mBridge; e-CNY over 95% |

## 8  Successes, Failures and Limits  (Slides 54–65)  — LO 7

- **What has worked:** Bitcoin (since 2009); Ethereum smart contracts and tokens (proof of stake since 2022); stablecoins (about $308 billion); tokenized Treasury funds (BlackRock's BUIDL about $2.7 billion; about $15 billion across roughly 100 products, trackers differ); Walmart traceability (Ch. 3); DTCC's live tokenized trades.
- **What the winners share:** an old, measurable problem; regulated incumbents; regulatory clarity before scale; a genuine need for a shared ledger.
- **ASX CHESS replacement:** ASX chose a distributed ledger from Digital Asset (announced 2015–16), scrapped the build in Nov. 2022 after an independent review and wrote off about A$250 million. It now pursues a phased non-DLT replacement; ASIC began court action in 2024.
- **Trade finance platforms:** we.trade (liquidation 2022), TradeLens (announced late 2022), Marco Polo (insolvency 2023) and Contour (ended Nov. 2023) failed mainly for lack of scale and funding.
- **Limits:** scalability, energy use, key management, irreversibility, regulatory uncertainty and interoperability.
- **Digital money in 2026:** retail CBDCs launched in the Bahamas, Jamaica and Nigeria with limited adoption; China reclassified e-CNY as deposit liabilities in Jan. 2026; the digital euro is in preparation (issuance possibly 2029); US retail CBDC work was halted in 2025 and the GENIUS Act created a payment stablecoin framework. Policy moves quickly; check current sources.

## Case Discussion Questions

Use these for discussion boards or Husky Folio reflections.

### DTCC tokenization

- Who benefits most from tokenizing DTC-held securities, and who might lose?
- Is a token on a public chain a share or a claim on a share? Which rights must travel with it?
- The base version gives no special collateral credit. Why might regulators start narrow?
- Compare this approach with the failed ASX project. What differed?

### ICO fraud and the SEC

- Why did counts of scams (78 percent) differ so much from dollars lost (about 11 percent)?
- Apply the Howey test to a token you could buy today. What facts matter?
- Why did Munchee matter even though no fraud was alleged?
- How did enforcement help or hurt legitimate blockchain projects?

### Cross-border payments

- Which of the four routes would you use for payroll to a supplier abroad? Why?
- What risks does a stablecoin carry that a bank tokenized deposit does not?
- Why does Swift keep final settlement on existing rails at first?
- How could ISO 20022 data help or limit any of the four routes?

## Key Terms

*Make sure you can define each of these in your own words. Tick the box when you can.*

| Ledgers, blocks, mining | Consensus, wallets, exchanges | Tokenization, ICOs, payments |
|---|---|---|
| ☐  digital ledger | ☐  consensus mechanism | ☐  tokenization |
| ☐  distributed ledger | ☐  proof of work | ☐  tokenized entitlement |
| ☐  node | ☐  proof of stake | ☐  digital twin |
| ☐  double spending | ☐  delegated proof of stake | ☐  no-action letter |
| ☐  public-key cryptography | ☐  proof of authority | ☐  DTC and DTCC |
| ☐  digital signature | ☐  Byzantine fault tolerance | ☐  CUSIP |
| ☐  genesis block | ☐  slashing | ☐  delivery versus payment |
| ☐  immutability | ☐  permissioned vs. permissionless | ☐  T+1 settlement |
| ☐  hash function | ☐  public blockchain | ☐  initial coin offering (ICO) |
| ☐  SHA-256 | ☐  private blockchain | ☐  white paper |
| ☐  avalanche effect | ☐  consortium blockchain | ☐  Howey test |
| ☐  block header | ☐  sidechain | ☐  exit scam |
| ☐  previous block hash | ☐  cryptoasset | ☐  unregistered offering |
| ☐  Merkle root | ☐  hot wallet | ☐  correspondent banking |
| ☐  nonce | ☐  cold wallet | ☐  tokenized deposit |
| ☐  difficulty target | ☐  multisig wallet | ☐  deposit token |
| ☐  mining | ☐  custodian | ☐  stablecoin |
| ☐  block reward | ☐  proof of reserves | ☐  GENIUS Act |
| ☐  halving | ☐  centralized exchange | ☐  XRP and RLUSD |
| ☐  transaction fee | ☐  decentralized exchange | ☐  ISO 20022 |
| ☐  block explorer | ☐  commingling | ☐  CBDC |
| ☐  block size and weight | ☐  smart contract | ☐  triple-entry accounting |

## Key Equations and Calculations

**Block reward at height h**

**R(h) = 50 ÷ 2^floor(h ÷ 210,000)  bitcoins**

Reward halves every 210,000 blocks. At h = 840,000: 50 ÷ 2^4 = 3.125. Summing all eras gives the 21 million coin cap.

**Expected mining attempts**

**Attempts ≈ 16^k  (k leading zero hex digits)**

Each hex digit multiplies the work by 16. Real Bitcoin compares the whole hash to a numeric target, so expected attempts ≈ 2^256 ÷ target.

**Difficulty retarget (every 2,016 blocks)**

**New difficulty = old difficulty × 20,160 ÷ actual minutes for the last 2,016 blocks**

20,160 minutes is 2,016 blocks at 10 minutes each (two weeks). Bitcoin limits one adjustment to a factor of 4.

**Merkle proof size**

**Hashes needed ≈ ⌈log₂(n)⌉**

A block with n transactions needs only about log₂(n) hashes to prove one transaction is included. For n = 3,334: 12 hashes.

**Throughput**

**TPS = transactions per block ÷ seconds per block**

Block 843,909: 3,334 ÷ 600 ≈ 5.6 transactions per second.

**Miner revenue and fee per transaction**

**Revenue = reward + fees;  fee per tx = total fees ÷ transactions**

Block 843,909: 3.125 + 0.1076 = 3.2326 BTC. Fee per transaction: 0.1076 ÷ 3,334 ≈ 0.0000323 BTC.

**Byzantine fault tolerance bound**

**n ≥ 3f + 1   (quorum = 2f + 1)**

To tolerate f faulty or malicious nodes you need at least 3f + 1 nodes. With 4 nodes, f = 1.

### Practice problems

1. What is the block reward at height 1,050,000?
2. The last 2,016 blocks took 12,960 minutes (9 days). By what factor does difficulty change?
3. How many hashes does a Merkle proof need for a block of 1,000 transactions?
4. A block holds 2,400 transactions and pays 0.09 BTC in fees. What is the fee per transaction?
5. How many nodes does a BFT network need to tolerate 2 malicious nodes?

*Answers are at the end of this guide.*

## Self-Check Questions

**1.  What did Haber and Stornetta contribute in 1991?**

- A) Proof-of-work consensus
- B) Cryptographically chained blocks to timestamp documents
- C) The first digital currency
- D) Public-key cryptography

**2.  What problem does a consensus mechanism primarily solve?**

- A) Encrypting data efficiently
- B) Agreeing on ledger state without a central authority
- C) Compressing data
- D) Speeding up card payments

**3.  What makes historic blockchain data tamper-evident?**

- A) Passwords on each block
- B) Nightly backups
- C) Each block contains the previous block's hash
- D) Encrypting every transaction

**4.  Which mechanism is energy-efficient but risks concentrating control among large holders?**

- A) Proof of work
- B) Proof of stake
- C) Proof of burn
- D) Proof of capacity

**5.  Which network type best fits interbank settlement among known banks?**

- A) Public, permissionless
- B) Private or consortium, permissioned
- C) Open to all retail users
- D) Proof-of-work public chain

**6.  What is triple-entry accounting?**

- A) Recording each entry three times
- B) Double-entry plus a shared cryptographic receipt
- C) A three-signature rule
- D) Tracking three currencies

**7.  What is the best lesson from the ASX CHESS replacement?**

- A) DLT projects in market infrastructure are quick
- B) Public chains are always better
- C) Complexity and governance in critical infrastructure are easy to underestimate
- D) Blockchain cannot settle securities

**8.  Which statement about throughput is most accurate?**

- A) Bitcoin and Visa process similar volumes
- B) Bitcoin's base layer handles a handful of transactions per second; Visa's capacity exceeds 65,000 messages per second
- C) Bitcoin handles 50,000 per second
- D) Visa is limited to 24,000 per second

**9.  Why can irreversibility be a problem in finance?**

- A) Errors can be easily undone
- B) Security is too weak
- C) Confirmed errors or fraud are hard to reverse
- D) All chains interoperate

**10.  How often does Bitcoin adjust mining difficulty?**

- A) Every block
- B) Every 2,016 blocks
- C) Every 210,000 blocks
- D) Every year

**11.  How often does the Bitcoin block reward halve?**

- A) Every 2,016 blocks
- B) Every 10 minutes
- C) Every 210,000 blocks
- D) Every 21 million blocks

**12.  A BFT network has 10 nodes. What is the largest number of faulty nodes it can tolerate?**

- A) 1
- B) 2
- C) 3
- D) 5

**13.  How does a deposit token differ from a typical stablecoin?**

- A) It is not on a blockchain
- B) It represents a claim on a bank deposit at the issuing bank
- C) It has no issuer
- D) It is always anonymous

**14.  What does a cryptocurrency wallet actually store?**

- A) The coins themselves
- B) The keys that control assets on the blockchain
- C) A copy of the whole ledger
- D) The exchange's order book

**15.  Which statement about the FTX collapse is supported by Chapter 3?**

- A) It was caused by a proof-of-work attack
- B) FTX and Alameda funds were blurred, triggering a run
- C) Regulators closed it before any withdrawals
- D) It had no connection to other firms

**16.  Which blockchain type is run by a group of firms and needs governance among members?**

- A) Public
- B) Consortium
- C) Proof of work
- D) Sidechain

**17.  What did the SEC staff no-action letter of December 2025 permit?**

- A) Trading any token on any exchange
- B) DTC to offer a defined tokenization service for three years
- C) Removal of T+1 settlement
- D) Unregistered ICOs

**18.  In the Nasdaq tokenization rule approved in March 2026, tokenized shares must have:**

- A) A different CUSIP
- B) The same CUSIP, symbol and shareholder rights
- C) No voting rights
- D) Separate order books

**19.  Which assets did the DTC pilot start with?**

- A) All crypto tokens
- B) Russell 1000 stocks, major index ETFs and Treasuries
- C) Private equity only
- D) Only government bonds of Europe

**20.  Which test did the SEC apply in its 2017 DAO Report?**

- A) Basel test
- B) Howey test
- C) Sarbanes-Oxley test
- D) Blue-sky test

**21.  The Satis Group found about 78 percent of 2017 ICOs were scams, but what share of dollars raised went to identified scams?**

- A) About 78 percent
- B) About 11 percent
- C) About 50 percent
- D) Nearly none

**22.  Why was the Munchee case notable?**

- A) It involved an exchange hack
- B) A so-called utility token was treated as a security
- C) It ended all ICOs
- D) It involved a central bank

**23.  How does Swift's shared ledger settle payments at first?**

- A) With a new Swift coin
- B) Banks' tokenized deposits are ordered on the ledger; final settlement uses existing systems
- C) With XRP
- D) With bitcoin

**24.  Which statement about GENIUS Act stablecoins is accurate?**

- A) They may pay interest
- B) Reserves must back each token 1:1; interest is barred
- C) They are central bank money
- D) They are unregulated

**25.  Which feature best explains why tokenization at DTC avoided major legal change?**

- A) It replaced the master securityholder file
- B) The token represents an entitlement; the record stays at DTC and securities law still applies
- C) It ended registration duties
- D) It removed custodians

## Hands-On Exercise: Hashing, Mining and Tampering in Python

This exercise uses only Python's standard library. Save the code below as hashdemo.py and run it from a terminal in the same folder with python hashdemo.py (or uv run hashdemo.py if you use uv).

```python
import hashlib

def sha256(text):
    return hashlib.sha256(text.encode()).hexdigest()

# Part A: the avalanche effect
print(sha256("FIN-4600 pays Bob 5"))
print(sha256("FIN-4600 pays Bob 6"))

# Part B: mine a toy block
def mine(data, zeros):
    nonce = 0
    prefix = "0" * zeros
    while True:
        digest = sha256(f"{data}{nonce}")
        if digest.startswith(prefix):
            return nonce + 1, digest   # attempts, hash
        nonce += 1

for zeros in (1, 2, 3, 4, 5):
    attempts, digest = mine("FIN-4600 block|", zeros)
    print(zeros, attempts, digest[:16])

# Part C: a three-block chain, then tamper with it
def build(transactions):
    chain, prev = [], "0" * 64
    for tx in transactions:
        block_hash = sha256(prev + tx)
        chain.append({"tx": tx, "prev": prev, "hash": block_hash})
        prev = block_hash
    return chain

def is_valid(chain):
    prev = "0" * 64
    for block in chain:
        expected = sha256(prev + block["tx"])
        if block["prev"] != prev or block["hash"] != expected:
            return False
        prev = block["hash"]
    return True

payments = ["Alice pays Bob 5", "Bob pays Carol 2", "Carol pays Dan 1"]
chain = build(payments)
print("Valid before tampering:", is_valid(chain))
chain[0]["tx"] = "Alice pays Bob 500"
print("Valid after tampering:", is_valid(chain))
```

### What to look for

- **Part A:** the two hashes share nothing even though the inputs differ by one digit.
- **Part B:** each extra leading zero multiplies the expected attempts by 16. Record your attempt counts for 1 to 5 zeros and compare with 16^k.
- **Part C:** changing one early transaction makes is_valid return False because every later link depends on it.
- **Extend it:** try 6 leading zeros and time it. Why does real Bitcoin use a numeric target instead of counting zeros?

## Videos

[3Blue1Brown, But how does bitcoin actually work?](https://www.youtube.com/watch?v=bBC-nXj3Ng4)

About 25 minutes. Builds a ledger, signatures and proof of work from scratch. Watch the proof-of-work section beginning near 14:38.

[3Blue1Brown, How secure is 256 bit security?](https://www.youtube.com/watch?v=S9JGmA5_unY)

Short companion that puts 2^256 in perspective for hash security.

[Anders Brownworth, Blockchain 101: A Visual Demo](https://www.youtube.com/watch?v=_160oMzblY8)

17:50. Chapters: SHA-256 hash 0:15, block 2:18, blockchain 5:16, distributed 9:20, tokens 12:19, coinbase transaction 14:36. Pair with the interactive demo.

[Anders Brownworth, Blockchain 101 Part 2: Public/Private Keys and Signing](https://www.youtube.com/watch?v=xIDL_akeras)

Shows how signatures authorize transactions; ties to the PGP lab.

[MIT OpenCourseWare, Gensler, Blockchain and Money, Session 1](https://www.youtube.com/watch?v=EH6vE97qIP4)

Finance-focused framing. Useful segments: what blockchain is (about 15:38) and financial sector problems and opportunities (about 26:40).

[Vitalik Buterin, Ethereum in 30 Minutes (ethereum.org)](https://ethereum.org/videos/ethereum-in-30-minutes-vitalik-buterin/)

Proof of stake, layer 2 scaling and where Ethereum is heading, from a co-founder.

## Primary and Web References

### Textbook

- [Nofsinger, Stein Smith & Bonaparte, Survey of Fintech, 2026 release (McGraw Hill): Chapters 2 and 3](https://www.mheducation.com/highered/product/survey-of-fintech-nofsinger.html)

### Foundational papers

- [Nakamoto (2008), Bitcoin: A Peer-to-Peer Electronic Cash System](https://bitcoin.org/bitcoin.pdf)
- [Diffie and Hellman (1976), New Directions in Cryptography](https://ee.stanford.edu/~hellman/publications/24.pdf)
- [Haber and Stornetta (1991), How to Time-Stamp a Digital Document](https://link.springer.com/article/10.1007/BF00196791)
- [Ethereum white paper](https://ethereum.org/whitepaper/)

### Standards and official guidance

- [NIST IR 8202, Blockchain Technology Overview](https://nvlpubs.nist.gov/nistpubs/ir/2018/NIST.IR.8202.pdf)
- [ISO 22739:2024, Blockchain and distributed ledger technologies: Vocabulary (ISO catalogue page)](https://www.iso.org/standard/82208.html)
- [BIS Annual Economic Report 2025, next-generation monetary and financial system](https://www.bis.org/publications/aer-2025/next-generation-monetary-financial-system)
- [ECB, digital euro project](https://www.ecb.europa.eu/euro/digital_euro/html/index.en.html)
- [Atlantic Council, Central Bank Digital Currency Tracker](https://www.atlanticcouncil.org/cbdctracker/)

### Technical documentation

- [Bitcoin developer guide](https://developer.bitcoin.org/devguide/)
- [Bitcoin Wiki, block hashing algorithm](https://en.bitcoin.it/wiki/Block_hashing_algorithm)
- [Ethereum.org, consensus mechanisms](https://ethereum.org/developers/docs/consensus-mechanisms/)
- [Ethereum.org, proof of stake](https://ethereum.org/developers/docs/consensus-mechanisms/pos/)
- [LF Decentralized Trust, Hyperledger Fabric v3.0 announcement (SmartBFT)](https://www.lfdecentralizedtrust.org/announcements/version-3.0-of-hyperledger-fabric-an-lf-decentralized-trust-project-now-available)
- [Princeton, Bitcoin and Cryptocurrency Technologies (book site)](https://bitcoinbook.cs.princeton.edu/)

### Case sources

- [ASX, CHESS replacement project page](https://www.asx.com.au/markets/clearing-and-settlement-services/chess-project)
- [J.P. Morgan Payments, JPM Coin USD deposit token for institutional clients](https://www.jpmorgan.com/payments/newsroom/jpm-coin-usd-deposit-token-institutional-clients)
- [Global Trade Review, Maersk and IBM end TradeLens](https://www.gtreview.com/news/top-stories/maersk-and-ibm-pull-the-plug-on-tradelens/)
- [MIT OpenCourseWare 15.S12, Blockchain and Money (full course)](https://ocw.mit.edu/courses/15-s12-blockchain-and-money-fall-2018/)

### Tokenization, ICOs and payments

- [SEC staff no-action letter to DTC (Dec. 11, 2025), PDF (may block automated tools)](https://www.sec.gov/files/tm/no-action/dtc-nal-121125.pdf)
- [DTCC, live production trades with tokenized assets (Jul. 2026)](https://www.dtcc.com/digital-assets/tokenization/live-production-trades)
- [DTCC, tokenization service update (May 4, 2026)](https://www.dtcc.com/news/2026/may/04/dtcc-advances-development-of-new-tokenization-service)
- [Ledger Insights, SEC approves Nasdaq tokenized trading](https://www.ledgerinsights.com/sec-approves-nasdaq-tokenized-trading/)
- [SEC press release 2020-146, Telegram settlement](https://www.sec.gov/news/press-release/2020-146)
- [Satis Group (2018), Cryptoasset Market Coverage Initiation: Network Creation](https://research.bloomberg.com/pub/res/d28giW28tf6G7T_Wr77aU0gDgFQ)
- [Swift, blockchain ledger ready for use (Jul. 9, 2026)](https://www.swift.com/news-events/press-releases/swifts-blockchain-ledger-ready-use-17-banks-set-pioneer-tokenised-cross-border-payments-trusted-global-infrastructure)
- [rwa.xyz, tokenized Treasuries tracker](https://app.rwa.xyz/treasuries)

### Live data and demos

- [mempool.space, Bitcoin block explorer](https://mempool.space)
- [Brownworth, interactive blockchain demo](https://andersbrownworth.com/blockchain/)

### Open-source repositories

Browse the code and the proposal repositories (BIPs and EIPs) to see how rules change in practice.

| Repository | What it is | Explore |
|---|---|---|
| [bitcoin/bitcoin](https://github.com/bitcoin/bitcoin) | Bitcoin Core reference client (C++) | Find the proof-of-work check and the 210,000-block halving constant. |
| [bitcoin/bips](https://github.com/bitcoin/bips) | Bitcoin Improvement Proposals | Read BIP 141 (Segregated Witness) to see why blocks can exceed 1 MB. |
| [ethereum/go-ethereum](https://github.com/ethereum/go-ethereum) | Geth, Ethereum execution client (Go) | Browse recent commits and release notes. |
| [ethereum/consensus-specs](https://github.com/ethereum/consensus-specs) | Executable specification of Ethereum proof of stake (Python) | Locate the slashing conditions. |
| [ethereum/EIPs](https://github.com/ethereum/EIPs) | Ethereum Improvement Proposals | Read the EIP that introduced fee burning (EIP-1559). |
| [anza-xyz/agave](https://github.com/anza-xyz/agave) | Solana validator client (Rust) | Compare its PoS plus Tower BFT design with Ethereum. |
| [IntersectMBO/cardano-node](https://github.com/IntersectMBO/cardano-node) | Cardano node (Haskell) | Note the research-driven development style. |
| [hyperledger/fabric](https://github.com/hyperledger/fabric) | Permissioned enterprise blockchain (Go) | Find the ordering service options: Raft and SmartBFT. |
| [corda/corda](https://github.com/corda/corda) | R3 Corda for financial agreements (Kotlin/Java) | Read how notaries provide consensus. |
| [bitcoinbook/bitcoinbook](https://github.com/bitcoinbook/bitcoinbook) | Mastering Bitcoin, open-source text | Read the chapter on mining and consensus. |
| [anders94/blockchain-demo](https://github.com/anders94/blockchain-demo) | Source code of the Brownworth browser demo | Run it locally and change the difficulty. |

## Common Mistakes

- **Blockchain is not Bitcoin.** Bitcoin is one application of the technology.
- **A hash is not encryption.** You cannot decrypt a hash; it is a one-way fingerprint.
- **Immutable does not mean correct.** A blockchain preserves whatever was written, including errors and fraud.
- **Mining is not solving a hard equation.** It is repeated guessing of a nonce until the hash meets the target.
- **Capacity is not average load.** Compare designs fairly when quoting transactions per second.
- **A token is not a new asset.** A tokenized security is the same security in a different record form, with the same legal duties.
- **A no-action letter is not a blanket approval.** It covers the specific service described, for a limited period.
- **Deposit tokens are not stablecoins.** A deposit token is a claim on a bank deposit; stablecoins are issued by non-bank or licensed issuers under separate rules.
- **A scam count is not a dollar count.** Most ICO scams were small; a few were huge.

## Answers

### Self-check questions

| Q | Answer | Why |
|---|---|---|
| 1 | B | They proposed linking timestamped records with hashes so backdating is detectable. Public-key cryptography is Diffie and Hellman (1976). |
| 2 | B | Nodes that do not trust each other must still agree on one ledger. |
| 3 | C | Altering a block changes its hash, which breaks every later link. |
| 4 | B | Selection odds rise with stake, so wealth can become influence. |
| 5 | B | Banks need participant control, privacy and compliance. |
| 6 | B | The third entry is a receipt both parties can verify on a shared ledger. |
| 7 | C | ASX scrapped the build in 2022 and wrote off about A$250 million. D is too absolute. |
| 8 | B | Capacity is a design figure, not average load, but the gap is large. |
| 9 | C | Card systems offer chargebacks; confirmed on-chain transfers generally do not. |
| 10 | B | The 2,016-block retarget keeps blocks near 10 minutes. |
| 11 | C | From 50 BTC in 2009 to 3.125 BTC after the 2024 halving. |
| 12 | C | n ≥ 3f + 1 gives f ≤ (10 − 1) ÷ 3 = 3. |
| 13 | B | JPMD is a tokenized representation of JPMorgan deposits, so the bank can still lend against them. |
| 14 | B | No cryptocurrency is held in a wallet; the keys control the assets recorded on the chain. |
| 15 | B | A CoinDesk report on Nov. 2, 2022 raised the issue, and a run followed; BlockFi and others were hurt. |
| 16 | B | Consortium chains are efficient for members but depend on trust and governance. |
| 17 | B | The letter authorized a specific DTC tokenization service for three years, with limits. |
| 18 | B | They trade on the same order book with the same priority and rights. |
| 19 | B | Scope was deliberately limited to highly liquid securities. |
| 20 | B | Howey asks about an investment of money in a common enterprise with profits from others' efforts. |
| 21 | B | About 1.3 of 11.9 billion dollars; a few huge scams dominated the dollars. It is an industry report, not an audit. |
| 22 | B | Marketing that promised value growth made the token an investment contract despite the utility label. |
| 23 | B | It orchestrates bank commitments 24/7 and complements, not replaces, existing rails. |
| 24 | B | The Act (July 2025) set one-for-one reserves and a licensing regime; final rules and enforcement phase in. |
| 25 | B | Tokenization wraps the existing entitlement; it does not change the legal framework. |

### Practice problems

| No. | Solution |
|---|---|
| 1 | floor(1,050,000 ÷ 210,000) = 5, so R = 50 ÷ 2^5 = 1.5625 BTC. |
| 2 | 20,160 ÷ 12,960 = 1.556, so difficulty rises about 55.6 percent. |
| 3 | ⌈log₂ 1000⌉ = 10 hashes. |
| 4 | 0.09 ÷ 2,400 = 0.0000375 BTC, which is 3,750 satoshis (1 BTC = 100,000,000 satoshis). |
| 5 | n ≥ 3(2) + 1 = 7 nodes. |

## Copyright and Sources

This guide paraphrases learning objectives, section topics, Table 2.1 and Table 3.1 from Nofsinger, Stein Smith & Bonaparte, Survey of Fintech, 2026 release, Chapters 2 and 3, for classroom instruction. Case updates come from the public sources listed above.

**© McGraw Hill LLC. All rights reserved. No reproduction or distribution without the prior written consent of McGraw Hill LLC.**
