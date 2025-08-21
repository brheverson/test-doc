# 

This document contains comprehensive notes from the third wave of PBA-x,
the online version of the Polkadot Blockchain Academy (PBA) program. The
primary objective is to transform raw notes from video lessons into
well-structured, readable content that serves as a definitive reference
guide for learners to refer to and review course material.

# Module: Introduction to Cryptography

## Cryptography Principles

### Cryptography overview

Cryptography brief historic and current use of cryptography. In the
past, the design of cryptography protocols used to be more based on
heuristics (relying on intuition) and the methods we designed mostly in
an ad hoc fashion. Nowadays a more scientific approach is used, we
design methods with well defined models that come with mathematical
proofs of security. The main idea is that the methods are guaranteed to
be secure in the specified model unless some underlaying assumption is
false.

Cryptography provides a collection of tools for secure interaction in
the presence of adversaries.

- public key encryption

- digital signatures

- zero knowledge proofs

- anonymous digital cache

- multi-party computation

### Secure communication

A user `A` wants to send a secure message `m` to user `B`.

In cryptography we need to specify what secure means in this case, in
other words we need to describe which security graranties are desired.
We need that because we do not want to start designing clever algorithms
without knowing the exact goals we want to achieve.

In this case security communication could means many things, for
instance:

- Confidentiality: the message `m` must remain secret (only the
  recipient must read).

- Authenticity: the recipient `B` is sure about the origin of message
  `m` is the sender `A`, and not anybody else.

- Non-repudiation: `A` cannot deny the fact that it sent the message `m`
  if he/she did it.

- (data) Integrity: if message `m` is tampered (modified/manipulated)
  with, if will be easily noticed.

- (data) Availability: not always considered as a cryptographic
  guarantee, but we want the data to be available to access.

- (data) Verifiability: in some contexts, we want to prove that possibly
  sensitive data satisfy some conditions/properties without revealing
  the information (zero knowledge proofs).

After defining these goals we have to think about how to achieve these
goals. This envolves typically specifying two points:

- Define the base layer we will start with:

- Specify the schemes or methods that make use of this base layer to
  achieve the goals.

In the case of the previous example of secure communication, we can for
instance, first start by defining an insecure communication channel over
the internet to allow `A` sending messages to `B`. Then, we speficy the
methods that may include encryption schemes, digital signatures and
other tools that will turn this insecure communication into a secure
channel that achieve the goals that we described in the begining.

#### Secure communication: an example

How A sends a secure message to B if the communication channel (cc) is
insecure.

<div class="informalexample">

`A` ------cc----→ `B`

</div>

First scenario, `A` knows `B` and both meet personally to exchange
copies of a key for a specific lock, then `A` uses this lock to lock the
message inside and send the box to `B`. `A` knows that only `B` can have
a key to open that lock. So the message is secured.

Now, lets assume that `A` and `B` can not meet each other, or they do
not know each other. How can they exchange the message securely now ?
`A` can actually get a box and send an unbraekable lock to `B`, `B` in
his turn gets the lock of `A` and puts his own unbreakable lock inside a
box and send back to `A`. `A` receives the lock of `B` which it can
remove from the box, and use the lock of `B` to lock the secure message
inside a box and send back to `B`, now locked with the lock of `B`, so
only `B` can open and read it.

This example shows a basic and simplified example of exchanging
cryptographic encryption keys.

### Kefckhoff’s Principle:

Created by Auguste Kerckhoffs in the 19th century.

The Kerckhoffs' principle states that the security of a cryptographic
system should not depend on the secrecy of the algorithm, but rather on
the secrecy of the cryptographic key. The system should remain secure
even if the algorith is publicly known, with the key being the only
confidential element.

In other words:

    A cryptosystem should be secure even if everything about the system is public knowledge, EXCEPT THE KEY.

We should not rely in the secrecy of the method, which is known as
security by obscurity. We should not hope that no one is able or capable
of figuring it out.

Reasons for that:

- It is easier to store a short key than the description of the method.

- if we loose a key it is easier to replace for a new one, than creating
  a new method.

- having the method public, allows for the community to work together to
  validate the method as a secure method.

Following Kerckhoffs principle allows for rigorous testing and analysis,
ensuring the security of the method or system that uses it.

Therefore, the power of cryptography methods relies on good choice of
keys. Keys that are random and unpredictable are harder to crack. We
measure the level of randomness of a key via entropy (the amount of
non-redundant information cointained, less random lower entropy).
Choosing keys from high entropy distributions, adversaries will have
lower probability of obtaining them.

### Take-away

- Criptography is today a core science for building secure and highly
  complex decentralized systems.

- We want to build systems with strong secutiry guarantess, with the
  smallest amount of assumptions as possible, and at the same time
  ensuring high performances. And this is an important trade-off.

- Understanding the basic primitives of cryptographic tools are core to
  achieve that.

## Hashing and Encryption

### Hash functions

Hash functions are one of the fundamental building blocks of
cryptography. A hash function is a one-way function that creates a
compact representation of input data. No matter how large the input, the
output—called the hash—is of fixed size. In other words, hash functions
map an unbounded input space to a bounded output space. They are
designed to be fast to compute in the forward direction. However,
reversing the process—recovering the input from the hash—is
computationally infeasible. Good hash functions are also
collision-resistant. That means it is very hard to find two different
inputs that produce the same hash. At their core, hash functions are
used to uniquely identify data.

#### Hash Function in Action

SHA-256 is a widely used hash function that belongs to the SHA-2 (Secure
Hash Algorithm 2) family. SHA-2 is a collection of cryptographic hash
functions designed for secure data hashing.

To see SHA-256 in action, we will use a visual demo available online.
The demo is hosted at
<https://guggero.github.io/blockchain-demo/#!/hash>.

It is an interactive tool that helps you understand how hash functions
behave with different inputs. This version is a complete rewrite of
Anders Brownworth’s original Blockchain Demo. While the underlying code
has changed, the core educational idea from Brownworth’s excellent demo
video remains.

Try changing the input data in the demo and observe how the hash output
changes. Even small changes in the input produce completely different
hashes. This property is known as the avalanche effect. It demonstrates
how hash functions are sensitive to input and useful for ensuring data
integrity.

#### Properties of Hash Funtions:

- accept unbounded size input

- map to a bounded fixed size output

- speed, the function must be quick to compute

- **an one-way function (pre-image resistence)**:

  - given a hash value, it should be computationally infeasible to find
    any input that produces that hash.

  - stops attackers from recovering the original input from the hash.

  - protects against reverse-engineering hashed passwords.

- **second Pre-Image Resistance**:

  - given a specific input and its hash, it should be computationally
    infeasible to find a different input that produces the same hash.

  - this is about modifying a known input without changing its hash.

  - used for Document tampering, signature forgery

- **collision resistence**: An attacker is free to find any two
  completely different files (arbitrary inputs) that hash to the same
  value.

  - this is especially dangerous for digital signatures, The attacker
    gets you to sign a benign file.

  - the famous SHA-1 collision attack (Google’s SHAttered project in
    2017)

**Key differences**: Freedom of Choice - Pre-image resistance: The
attacker is stuck with a specific hash. - Second pre-image resistance:
The attacker is stuck with a specific input. - Collision resistance: The
attacker is free to choose everything.

Weak collision resistance, which is also called second preimage
resistance. Strong collision resistance, is also known as Collision
resistence.

#### Summary

| Hash Property | Attack Example | Real-World Risk |
|----|----|----|
| Pre-Image Resistance | Recover original input from a known hash (e.g., cracking a hashed password) | Hash reversal (password recovery, leaking sensitive data) |
| Second Pre-Image Resistance | Given a signed document, find a **different** document with the same hash | Document tampering (targeted forgery of signed documents) |
| Collision Resistance | Create two distinct documents that hash to the same value before signing | Signature forgery (swap benign document with malicious one after signing) |

### Hash Functions: Cryptographic vs Non-cryptographic

There are 2 types:

- **Cryptographic hash functions**: provide security guarantess and are
  supposed to be collision resistent slower than non-cryptographic ones,
  but used when security is a primary concern

- **Non-cryptographic hash functions**: much faster, but weeker
  guarantees, used in file retrival and indexing, can also be used when
  users/nodes are known to NOT be malicious.

> [!NOTE]
> It is important to rememnber that no hash function is fully collision
> resistant, there’s always a possibility of a collusion given the fact
> that the input domain is larger than the output range. This is a
> fundamental concept known as the **Pigeonhole Principle**, which
> guarantees that collisions must exist for any hash function with a
> fixed output size.

**Why Collisions Are Inevitable**?

The rationale behind that:

    The reason collisions must happen is based on basic math and logic.
    A hash function takes inputs that can be infinitely large or very big.
    But it produces outputs of fixed size, for example, 256 bits.
    This means there are only a finite number of possible outputs—specifically, 2^256.
    Since the input space is much larger than the output space, some inputs must share the same output.
    This situation is known as a collision.
    You can think of it like having 10 pigeonholes but 11 pigeons.
    With more pigeons than holes, at least one hole must contain two pigeons.
    Similarly, multiple inputs must map to the same hash output.

So, Collision resistance does not mean no collisions exist.

- Instead, it means it is computationally infeasible to find such
  collisions.

- In other words, finding a collision should require an impractical
  amount of time and resources (far beyond current computing
  capabilities).

If collisions become easy to find (like SHA-1 today), the hash function
is considered broken for cryptographic use and should be replaced by
stronger functions (SHA-256, SHA-3, etc.).

### Hash Function: Applications

Hash functions also provide data integrity. Even a single bit change in
the input causes a completely different hash output. This makes it easy
to detect any tampering with the data.

Hash functions can also be used as commitment schemes. A commitment
scheme binds input data to a specific output, providing strong privacy
guarantees. This means the output hides all information about the input.
With a commitment scheme, you can prove you committed to some data
without revealing it.

We will see commitment schemes in more detail later.

#### Take-away

- A hash function is one of the most fundamental building blocks in
  cryptography.

- Hash functions produce fixed-size outputs from inputs of any size.

- They are designed to be fast to compute and hard to reverse.

- Hash functions ensure data integrity by producing drastically
  different outputs for small input changes.

- They are essential for securing data and verifying identities.

- Hash functions play a critical role in blockchain technology to ensure
  transaction integrity and security.

### Commitment schemes

A commitment scheme is a cryptographic primitive that allows one to
commit to a chosen value (or chosen statement) while keeping it hidden
to others, with the ability to reveal the committed value later.

In other words, a commitment scheme is a way to "lock" a secret value.
It lets you commit to a value without revealing it right away. Later,
you can "open" the commitment to prove what the original value was.

So, it lets you:

- **Commit**: Lock in a value secretly (put it in the envelope).

- **Reveal**: Later, open the envelope to reveal the committed value.

And ensures two important properties:

- **Hiding:** No one can guess the committed value before you reveal it.

- **Binding:** You cannot change the value after committing.

Commitment schemes are like putting a message in a locked box. You show
the box to others but keep the message secret. When ready, you open the
box and reveal the message. This helps in secure protocols where privacy
and honesty matter. Hash functions are often used to build commitment
schemes.

#### How Hash Functions Implement Commitment

You can use a hash function to build a simple commitment scheme.

##### Commit Phase

- Choose a value `x` that you want to commit to.

- Pick a random value `r` (called a nonce) to keep it secure.

- Compute the commitment: `C = H(x | r)` where `|` means concatenation.

- Share `C` as your commitment. Keep `x` and `r` secret for now.

##### Reveal Phase

- When you’re ready, reveal both `x` and `r`.

- Anyone can verify your commitment by checking: `H(x | r) == C`

- If the values match, the commitment is valid.

##### Why It Works: Required Properties

- **Hiding:** The random `r` hides `x`. Without `r`, no one can guess
  `x` from `C`. This assumes the hash behaves like a random oracle.

- **Binding:** Because of collision resistance, you can’t find other
  values `x'`, `r'` such that `H(x' | r') == H(x | r) == C` unless they
  are the same as `x` and `r`. This means you can’t change your
  committed value later.

Commitment schemes built from hash functions are simple but powerful.
They are used in many cryptographic protocols to ensure fairness and
privacy.

##### Real-World Use Cases

Commitment schemes are used in many real-world cryptographic systems.

- **Secure Auctions:** Bidders commit to their bids in secret. Later,
  they reveal the bids. This prevents cheating or changing bids after
  seeing others.

- **Zero-Knowledge Proofs:** Commitments hide secret values. You can
  prove something is true without showing the secret itself.

- **Blockchain:** Commitments are used to record transactions or states.
  Once committed, the data cannot be changed without detection.

- **Digital Contracts:** Parties can commit to contract terms before
  revealing them. This adds fairness and prevents manipulation.

These use cases show how commitments provide both privacy and trust.

### Real-World Use Cases

Commitment schemes are used in many real-world cryptographic systems.

- **Secure Auctions:** Bidders commit to their bids in secret. Later,
  they reveal the bids. This prevents cheating or changing bids after
  seeing others.

- **Zero-Knowledge Proofs:** Commitments hide secret values. You can
  prove something is true without showing the secret itself.

- **Blockchain:** Commitments are used to record transactions or states.
  Once committed, the data cannot be changed without detection.

- **Digital Contracts:** Parties can commit to contract terms before
  revealing them. This adds fairness and prevents manipulation.

These use cases show how commitments provide both privacy and trust.

### Encryption: Symmetric vs Asymmetric

Encryption is the process of transforming readable data (plaintext) into
an unreadable form (ciphertext) using a cryptographic key. Only someone
with the correct key can convert the ciphertext back into the original
plaintext.

There are two main types of encryption: symmetric and asymmetric.

#### Symmetric Encryption

- Also called secret-key encryption.

- The same key is used for both encryption and decryption.

- Both the sender and receiver must share this key in advance.

- This can be difficult if the parties have never met or don’t already
  trust each other.

Examples: ChaCha20, AES, DES, Blowfish, Twofish, Serpent

##### Guarantees

**Provides:**

- Confidentiality (keeps data secret)

**Does NOT provide:**

- Integrity (detecting tampering)

- Authenticity (proving who sent the message)

- Non-repudiation (preventing denial of sending)

#### Asymmetric Encryption

- Also called public-key encryption.

- Uses a key pair: one public, one private.

- The public key encrypts the data.

- Only the matching private key can decrypt it.

- The public key can be shared openly.

- The private key must be kept secret.

- Asymmetric encryption is more computationally expensive than
  symmetric.

- It is not ideal for encrypting large amounts of data.

A common use is secure key exchange. Two parties can exchange a
symmetric key using asymmetric encryption. Then they switch to symmetric
encryption for ongoing communication. This gives the speed of symmetric
encryption with the security of asymmetric key setup.

Examples: RSA, ElGamal, Paillier

##### Guarantees

**Provides:**

- Confidentiality (same as symmetric)

**Does NOT provide:**

- Integrity

- Authenticity

- Non-repudiation

### Assymetric Encryption: Explained Example

Two breakthrough algorithms that allow secure communication between two
parties without a shared secret:

- RSA algorithm (1977): Relies on number theory, specially in prime
  numbers and the difficulty of prime number factorization. It provides
  a public/private key pair which are really long numbers. Then if two
  parties want to communicate securely the first sends it public key to
  the other. Upon receipt the public key it uses the public key to
  encrypt the message, which that only the owner of the public key can
  read the message. And this is what we call assymetric cryptography and
  this is computationally expansive.

- Diffie-Hellman key exchange algorithm (1976/77): it is a mathematical
  method of securely generating a symmetric cryptographic key over a
  public channel and was one of the first protocols as conceived by
  Ralph Merkle and named after Whitfield Diffie and Martin Hellman.

Let’s see an example:

<div class="informalexample">

A and B want to exchange messages

</div>

**Parameters**: These values are known to everyone (including
attackers):

- p: A large prime number (modulus)

- g: A primitive root modulo p (called the base or generator)

For instance, p = 23, g = 5

- **Alice generates**:

  - A secret random number a

  - Computes A = g^a \mod p

  - Sends A to Bob

    - Message from Alice to Bob: A = g^a mod p

- **Bob generates**:

  - A secret random number b

  - Computes B = g^b \mod p

  - Sends B to Alice

    - Message from Bob to Alice: B = g^b mod p

- **Both compute the shared secret**:

  - Alice receives B, computes: S = B^a \mod p = g^{ba} \mod p

  - Bob receives A, computes: S = A^b \mod p = g^{ab} \mod p

Now both share the same secret S, without having sent it directly.

Note:

- The shared key is never sent, only derived.

- DH by itself provides key agreement, not encryption or authentication.

- It’s vulnerable to man-in-the-middle attacks unless combined with
  authentication (like in TLS).

### Take-away

- Encryption protects the confidentiality of sensitive data.

- There are two main types: symmetric and asymmetric encryption.

- Symmetric encryption is fast and efficient.

- But it requires both parties to share a secret key in advance.

- Asymmetric encryption solves the key exchange problem.

- It uses a public key to encrypt and a private key to decrypt.

- However, asymmetric encryption is much slower.

- In practice, both methods are often combined for secure and efficient
  communication.

### References

- <https://en.wikipedia.org/wiki/Public-key_cryptography>

- <https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange>

- <https://cryptotools.net/rsagen>

- <https://polkadot-blockchain-academy.github.io/pba-content/singapore-2024/syllabus/1-Cryptography/4-Encryption-slides.html#/9>

## Accounts and Keys

### Wallets and Accounts

A wallet stores the cryptographic keys needed to access and manage
digital assets on the blockchain. It doesn’t actually hold the
cryptocurrency itself, but instead facilitates the secure storage and
management of the keys required to carry out transactions. They are
known to be complicated even for technical people.

### Accounts and Self Custody

Creating a blockchain account is a simple process, we just need to have
a crypto wallet.

Usually while creating an account we are given a 12 words pass phrase
known as seed phrase or recovery phrase. The 12-word phrase is a way to
securely store your private key in a format that’s easy to write down
and remember. It can recreate your wallet on any compatible wallet app
or device.

This seed phrase is an implementation of the `BIP-39` standard, which
converts a randomly generated number (the private key seed) into a list
of 12, 18, or 24 simple words from a predefined list.

This makes it easier to back up and reduces human error compared to
copying a long hexadecimal string. It can be used across different
wallets that follow the same standard, meaning you’re not locked into
one wallet provider.

If someone has your seed phrase, they own your wallet. It’s more
important than your password or device. If you lose your seed phrase,
you lose access to your funds forever. There is no `reset` option in
decentralized systems.

**Types of wallets**

- **Hot Wallet**: A hot wallet is connected to the internet, allowing
  for easy and quick access to your digital assets. They are typically
  used for daily transactions and trading due to their convenience.
  Example: MetaMask

- **Cold Wallet**: A cold wallet is not connected to the internet,
  offering better security for the long-term storage of digital assets.
  These are ideal for securely storing assets that aren’t needed for
  daily transactions. Example: Ledger

#### Cryptographic keys - Encoding

Computers work using binary data (long strings of 0s and 1s). But these
raw binary strings are very hard for humans to read, write, or share
without making mistakes. To solve this we use encoding, a way to
represent binary data in a format that is more readable and manageable
for humans.

Common Encoding Types in Cryptography & Blockchains

- Hexadecimal (Base16)

  - Widely used to represent hashes and keys in blockchains and
    cryptography.

  - Uses 16 characters (0-9 and A-F) to represent binary data in a
    compact, readable way.

<div class="example">

<div class="title">

A SHA-256 hash looks like

</div>

3a3f8b8d65e6a6b7f3e9c2f4a7e5b6d4e3a3f9gha6e7c3d4a5f3b2e1d0a8c9b7f6

</div>

- Base64 (General Purpose, Not Often Used in Bitcoin)

  - Very compact but can include confusing characters like +, /, and =.

  - Extremely useful in other contexts where compact binary-to-text
    encoding is needed.

Examples: sending images or attachments in emails (MIME format),
embedding small images in HTML or CSS as data:image/png;base64

- Base58 (Used for Bitcoin Addresses)

  - Bitcoin addresses are encoded in Base58 to make them:

    - Shorter than hexadecimal

    - Easier for humans to read by removing similar-looking characters,
      it excludes: 0 (zero), O (capital o), I (capital i), and l
      (lowercase L)

<div class="example">

<div class="title">

Bitcoin address looks like

</div>

1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa

</div>

**Take-away**:

Why Encoding Matters

- Human readability: Easier to write down, share, and verify.

- Error reduction: Base58 specifically avoids characters that are
  commonly misread.

- Data integrity: Encoded strings can still be verified
  cryptographically even if they are human-friendly.

#### Cryptographic keys - Mnemonics

The BIP-39 standard is a mnemonic system specifically designed for
creating and recovering cryptographic keys in a human-readable way. A
mnemonic system helps people remember or record complex information more
easily.

BIP-39 stands for Bitcoin Improvement Proposal 39. It defines: - How to
create mnemonic phrases (like the 12, 18, or 24-word passphrases). - How
to convert those phrases into a cryptographic seed that can generate
private keys and addresses.

<div class="example">

<div class="title">

a BIP-39 12-word seed

</div>

abandon ability able about above absent absorb abstract absurd abuse
access accident

</div>

Substrate uses the dictionary of the `BIP39` standard.

#### Cryptographic keys - Address Format

In the context of cryptography, an address is essentially a public key.
In cryptocurrencies when talk about keys, usually we are talking about
an address.

Addresses also include error check machanisms like checksum. It is a
simple however efficient way of checking if there was an error while
copying or sendind an address information. (if a single bit is wrong,
the checksum will inform that the address is invalid).

Polkadot uses a specific address format, called SS58 which uses Base68
encoding and adds 2 bytes at the beginning of the address to indicate
the network. It also includes 2 bytes of checksum at the end. SS58
Address format is designed keeping the multichain nature of Polkadot in
mind.

You can use
[format-transform](https://polkadot.subscan.io/tools/format_transform)
tool to check the account address on the other chains in the Polkadot
ecosystem. There has been a significant debate within Polkadot ecosystem
to unify the address format across chains. You can see the discussion
[here](https://forum.polkadot.network/t/unifying-polkadot-ecosystem-address-format/10042).

#### Account Derivation Methods

Starting Point: Mnemonic Phrase, this is our master seed.

From this single seed, you can generate multiple accounts (each with
their own public/private key pairs). This process is called
\*Hierarchical Deterministic (HD) Wallet Derivation. So, Key derivation
allows one to derive (virtually limitless) child keys from one "parent".

**Why do we need account derivation?**

- You don’t need a new mnemonic every time you want a new account.

- You can generate unlimited accounts from one passphrase.

- All accounts can be recovered from the same seed phrase.

- Keeps backup simple — one phrase to secure everything.

**How It Works ?**

1.  The 12-word phrase is converted into a master seed (a big random
    number).

2.  The seed generates the first private key (root key).

3.  Using derivation paths, you can systematically generate a new
    public/private keypair.

4.  Each Account is Unique: different keys, different addresses — but
    all are mathematically connected to the same mnemonic root

<div class="formalpara">

<div class="title">

Each step is a new account (new public/private keypair).

</div>

    Account 0: m/44'/0'/0'/0/0
    Account 1: m/44'/0'/0'/0/1
    Account 2: m/44'/0'/0'/0/2

</div>

**Types**

There are two types of key derivation: Hard and Soft keys deivation.

- **Soft** (Non-Hardened) derivation:

  - Allows public key derivation using a parent public key.

  - You can derive child public keys from the parent public key alone
    (no need for the private key).

  - You cannot generate a child private key only with the parent public
    key.

  - You can derive BOTH child public and private keys from the parent
    private key.

  - Weakness: If someone has parent public key + any child private key →
    they can compute the parent private key.

- **Hard** (hardnened) derivation:

  - You ALWAYS need the parent private key to derive anything.

  - From parent private key → can derive both child private keys and
    child public keys.

  - From parent public key → cannot derive anything (not even child
    public keys).

  - Safer: Even if someone has a child private key and the parent public
    key → they cannot compute the parent private key.

**Use cases**

**Soft Derivation** : Convenience & Public Monitoring - Watch-Only
Wallets (Public View Wallets): You want to monitor incoming transactions
without having access to private keys. Ex: Payment gateways, Block
explorers, - Lightweight Clients: Mobile or web wallets that want to
generate public addresses quickly and safely. - Payment Servers / POS
Systems: A point-of-sale system that needs to generate fresh addresses
for each customer, The POS system can derive child public keys without
ever handling private keys.

**Hard Derivation** : Security & Privacy Protection - Securing Master
Private Keys: When you want to make sure that if a child private key is
exposed, the master private key is still safe. - Account Separation in
Multi-Account Wallets: Different accounts for different users or
purposes. - Cold Storage Setups: Cold wallets for long-term storage.
ensures that even if a receiving address private key is accidentally
exposed, the whole wallet seed and other accounts are still safe. -
Multi-Signature Wallets (Partially Hardened): Wallets where multiple
parties must sign transactions, ensure that shared public keys don’t
leak sensitive structure or parent keys.

#### Activity: practical example of hard and soft key derivation

An example and activity, we use Polkadot JS Extension as it provides
ability to create and derive accounts using a mnemonic phrase. Note:
[Polkadot JS Extension](https://polkadot.js.org/extension/) is for
developers only. If you are a developer and would like to explore the
Polkadot account generation process, check the `Subkey` tool.

The article mentioned in the video: "How likely is it that someone could
guess your Bitcoin private key?"

## Digital Signatures

### Properties

- ensures authenticity and integrity of digital communication, without
  reveling the private key.

- authenticity: the message originates from the sender that claims to be
  the sender

- integrity: the message was not altered during communication.

Signature libraries should expose the following basic functions:

- `generate_key(r) → sk` : generate a secret key (sk) from some 'r'
  input.

- \`public_key(sk) → pk \` : generate a public key (pk) from sk.

- `sign(sk, m) → sig` : takes sk and a message 'm' and returns a digital
  signature.

- `verify(pk, m, sig) → bool` : takes sk, m, and a signature and returns
  true if the signature is valid. false otherwise.

Note that, is often more practical to sign the hash of a message,
instead of the full message, therefore sign and verify functions usually
have the following forms instead:

- `sign(sk, H(m)) → sib`

- `verify(pk, H(m), sig) → bool`

Other properties of signatures:

- Non-repudiation: Yes, The sender cannot deny sending the message.

- Confidentiality: No, weak [^1] or absent because:

  - The signature process does not hide the message.

  - The signed message remains readable by everyone.

  - The purpose is to verify authenticity, not to conceal content.

### Digital signatures in Practice

As just mentioned, in practice only the hash of a message is required to
sign the message and verify the signature. One challenge that signatures
must address are replay attacks. In a replay attack, the attacker
intecepts the message and resends it to manipulate the system. For
instance, imagine that an attacker intercepts a message that transfer
some amount of assets from account A to account B. It replays (resends)
this message multiple times in order to make multiple unauthorized
transfers.

To prevent replay attacks, digital signatures usually implement
additional information, such as nonces, timestamps or context
information.

- **Nonces** (Number used once): A unique, random value added to each
  message. Prevents duplication since the server checks whether the
  nonce has been used before.

- **Timestamps**: Ensure the message is only valid within a specific
  time window. Prevents old messages from being reused.

- **Context Information**: includes any data that defines the
  environment or circumstances of a transaction. Examples: session ID,
  timestamp, sender ID, location, protocol version, etc. For instance,
  we can bind signatures to specific sessions, so they cannot be reused
  in a different context.

### Digital Signatures in Action

This section shows a video of showing a transaction being sent from user
A to B, the highlight of this example is to show the incremental nonce
added to the transaction by the wallet as well as the timestamp that
defined the lifetime that is added by the call. Both are measure of
preventing replay attacks.

### Digital Signatures - Elliptic curves

There are several algorithms used to create digital signatures, each one
with its strengths and use cases. Some of the most prominent include:

#### ECDSA (Elliptic Curve Digital Signature Algorithm)

ECDSA is a widely used digital signature scheme based on elliptic curve
cryptography (ECC). It provides a way to verify the authenticity and
integrity of digital messages or data.

Efficiency: ECDSA offers strong security with smaller key sizes compared
to RSA, making it faster and more suitable for constrained environments.
Non-Deterministic: ECDSA relies on generating a random nonce for each
signature; if the nonce is reused or predictable, it can compromise the
private key. Usage: Common in Bitcoin, Ethereum, TLS, and many
cryptographic protocols.

Limitations of ECDSA:

- Nonce Sensitivity:

  - Biggest Weakness: ECDSA critically depends on using a unique,
    high-quality random nonce (per signature).

  - Risk: If the same nonce is reused, or if the nonce is partially
    predictable, the private key can be fully recovered.

  - Example: Several real-world attacks (including on Bitcoin wallets)
    have exploited poor nonce generation to steal private keys.

- Non-Deterministic by Default:

  - ECDSA traditionally relies on external randomness for each
    signature, which can introduce security risks if the random number
    generator is flawed.

  - Deterministic variants (like RFC 6979) have been proposed but are
    not always implemented.

- Complex Implementation

  - ECDSA is more mathematically complex than some alternatives like
    EdDSA, making it harder to implement securely and efficiently.

  - More prone to side-channel attacks if not carefully designed.

- Slower than Modern Alternatives:

  - ECDSA is generally slower than Ed25519, both in signing and
    verifying, especially on constrained devices.

- Larger Signature Size Compared to Ed25519:

  - While smaller than RSA, ECDSA signatures are typically larger and
    slower to verify than Ed25519, which has become a more preferred
    standard in modern cryptography.

#### Ed25519 (Edwards-curve Digital Signature Algorithm)

Ed25519 is a high-performance, modern digital signature algorithm that
implements the Edwards-curve Digital Signature Algorithm using the
Edwards curve Ed25519.

- It is designed to provide fast, secure, and compact signatures with
  strong resistance to common cryptographic attacks.

- Deterministic Signatures: Eliminates nonce-related vulnerabilities by
  generating nonces deterministically from the private key and message.

- High Performance: Faster signing and verification compared to ECDSA,
  especially on low-power or embedded devices.

- Compact Size: Public keys are 32 bytes and signatures are 64 bytes,
  which saves storage and bandwidth.

- Side-Channel Resistance: Designed to minimize the risk of timing and
  side-channel attacks.

- Widely Adopted: Used in modern systems like SSH, TLS 1.3,
  cryptocurrencies (like Monero), and secure messaging apps

- Ed25519 is inspired by Schnorr signatures but is a more advanced and
  standardized form, adapted to use modern elliptic curve techniques and
  deterministic signing.

Limitations of Ed25519:

- Curve Rigidity

  - Ed25519 uses a fixed curve (the Edwards form of Curve25519).

  - Unlike ECDSA, which can work over various NIST curves, Ed25519
    offers no flexibility to choose different elliptic curves.

- Limited Support for Advanced Features

  - ECDSA can be more easily adapted to schemes like threshold
    signatures or certain multi-signature protocols.

  - Ed25519 requires additional construction (like MuSig2 or other
    schemes) for efficient multi-signature set

#### Sr25519 (Schnorrkel/Ristretto x25519):

Sr25519 is a modern digital signature scheme based on Schnorr signatures
using the Ristretto group, which is derived from Curve25519.

It is specifically designed for use in modern blockchain platforms like
Polkadot and Substrate.

- Based on Schnorr Signatures: Offers simple security proofs and strong
  resistance to common cryptographic attacks.

- Uses Ristretto: This solves the cofactor issues of elliptic curves and
  provides a clean, prime-order group for better cryptographic
  properties.

- Deterministic Signatures: Like Ed25519, Sr25519 uses deterministic
  nonce generation, avoiding the nonce reuse problems of ECDSA.

- Batch Verification: Supports efficient batch signature verification,
  improving performance when validating many signatures at once.

- Native Support for multi-signature schemes, zero-knowledge-friendly
  constructs.

- Optimized for Blockchain: Specifically built for high-performance,
  scalable blockchain systems.

Limitations of Sr25519

- Limited Adoption Outside Specific Ecosystems: Sr25519 is currently
  most widely used in the Polkadot ecosystem. Broader adoption in
  standards like TLS, SSH, or legacy systems is still very limited.

- Complexity Compared to Ed25519: The Ristretto abstraction adds a layer
  of complexity that makes Sr25519 slightly harder to understand and
  implement from scratch compared to Ed25519.

- Less Mature Tooling: Although rapidly improving, Sr25519 has less
  mature libraries and tooling across all platforms compared to Ed25519
  or ECDSA. Integration in multi-language environments is still catching
  up.

- Larger Signature Size Than ECDSA: Sr25519 signatures are similar in
  size to Ed25519 (64 bytes), which is larger than the smallest possible
  ECDSA signatures when using certain curves and optimizations.

#### Summary table

| Feature | ECDSA | Ed25519 | Sr25519 |
|----|----|----|----|
| Cryptographic Basis | Elliptic Curve Digital Signature Algorithm (ECDSA) | Edwards-curve Digital Signature Algorithm (EdDSA) | Schnorr Signatures over Ristretto (based on Curve25519) |
| Deterministic Signatures | No (requires secure random nonce) | Yes (deterministic nonce from message and private key) | Yes (deterministic nonce from message and private key) |
| Nonce Sensitivity | High (reuse can leak private key) | Low (deterministic nonce generation) | Low (deterministic nonce generation) |
| Key Size | Typically 32 bytes (256-bit curve) | 32 bytes | 32 bytes |
| Signature Size | Variable, often ~70 bytes | 64 bytes | 64 bytes |
| Performance | Moderate (slower than Ed25519/Sr25519) | Very fast (optimized for speed) | Fast, with efficient batch verification |
| Batch Verification | Less efficient | Supported | Highly efficient |
| Multi-signature Support | Requires custom protocols | Possible but complex | Natively supported, more efficient |
| Side-Channel Resistance | Prone if not carefully implemented | Designed to resist side-channel attacks | Designed to resist side-channel attacks |
| Ecosystem Adoption | Widely used (Bitcoin, Ethereum, TLS, legacy systems) | Widely used (SSH, TLS 1.3, modern systems) | Primarily in Polkadot, Substrate, emerging adoption |
| Tooling & Libraries | Mature, broadly available | Mature, broadly available | Growing, but less mature across all platforms |
| Flexibility of Curve Choice | Multiple curves (e.g., secp256k1, P-256) | Fixed to Ed25519 | Fixed to Ristretto derived from Curve25519 |
| Use Cases | Traditional financial systems, blockchain, secure communication | Modern secure systems, SSH, cryptocurrencies, messaging apps | Blockchain (Polkadot, Substrate), scalable decentralized systems |

### Multi-Signatures

Multi-signature (multi-sig) is a cryptographic technique that requires
multiple parties to jointly authorize a transaction or message. Instead
of relying on a single private key, a group of keys must collaborate to
produce a valid signature.

- A multi-sig account is controlled by a set of individuals that hold
  each their own pub/priv key pairs.

- The multi-sig account have only a public key, but not a private key.

- Funds can be sent to the public address of the multi-sig account and
  they can only be moved out if there is a preset threshold of
  signatures received.

<div class="informalexample">

A “2-of-3” multi-sig wallet requires any 2 out of 3 participants to
approve a transaction before it can be executed.

</div>

**Advantages**

- Enhanced Security: Reduces risk of single key compromise.

- Shared Control: Useful for organizations, families, or teams to manage
  joint assets.

- Trustless Collaboration: No need for a central authority; signatures
  can be verified collectively.

- Resistance to Theft: Even if one key is stolen, funds or access remain
  secure.

**Use Cases** - Cryptocurrency Wallets: Bitcoin, Ethereum, Polkadot
(shared wallets with multiple owners). - Decentralized Governance:
Multi-party control of treasury funds. - Escrow Services: Conditional
transactions requiring multiple approvals. - Cross-Chain Bridges: Secure
multi-party authorization for asset transfers between blockchains. -
Enterprise Access Control: Secure corporate signing processes.

**Limitations**

- Complexity: More complicated setup and key management.

- Higher Transaction Fees: Multi-sig transactions can be larger and more
  expensive on-chain (notably in Bitcoin).

- Limited Privacy: Traditional multi-sig (like Bitcoin’s) can reveal the
  number of participants and threshold on the blockchain.

- Interoperability: Not all wallets, smart contracts, or protocols
  natively support multi-sig.

- Recovery Challenges: Loss of multiple keys can permanently lock
  access.

**Anonimity/Privacy**

The anonymity of signers in a multi-signature group depends heavily on
the type of multi-sig scheme used. It is possible to check each of the
members of a multi-sig group has signed a message. But there are some
multi-sig schemes that preserv the anonimity of the signers. For
instance Ring signatures preserve the anonumity.

- Traditional Multi-Sig (e.g., Bitcoin P2SH)

  - Visibility: The multi-sig structure is publicly visible on the
    blockchain.

  - Low anonymity – signers and signing policies are exposed.

- MuSig and MuSig2 (Schnorr-based Multi-Sig)

  - All participant keys are aggregated into a single public key.

  - The resulting signature is indistinguishable from a single-signer
    signature.

  - High anonymity – no information about the number of participants or
    individual signers is revealed.

- Threshold Signature Schemes (TSS)

  - TSS allows a group to jointly compute a signature without ever
    revealing individual key shares.

  - To outside observers, the signature looks like it was produced by a
    single key.

  - Strong anonymity – individual signers remain hidden, no signer
    metadata is leaked

- Smart Contract Multi-Sig (e.g., Gnosis Safe on Ethereum)

  - The contract publicly records which keys are authorized and which
    keys signed each transaction.

  - Low to medium anonymity – signer identities and approval history are
    usually on-chain.

#### Quick comparation table

| Scheme | Signer Anonymity | Requires Group Manager | Notes | Common Use |
|----|----|----|----|----|
| Aggregated Signatures | Low (unless using MuSig2) | No | Compress multiple signatures into one | Blockchain consensus, BLS signatures |
| Ring Signatures | Strong | No | Untraceable, signer hidden within group | Privacy coins (e.g., Monero), anonymous reporting |
| Group Signatures | Conditional | Yes (optional traceability) | Anonymity with controlled accountability | Corporate voting, permissioned blockchains |
| Threshold Signatures | Varies (can be anonymous) | No | Distributed signing, no single key exposure | Custodial wallets, distributed ledgers, cross-chain bridges |

**Tools and Protocols**:

- Bitcoin:

  - Native P2SH (Pay-to-Script-Hash) multi-sig

  - MuSig and MuSig2 (next-gen Schnorr-based multi-sig with privacy and
    efficiency improvements)

- Ethereum:

  - Gnosis Safe

  - Smart contract multi-sig wallet, industry standard

- Polkadot/Substrate:

  - Native multi-sig support via Sr25519

- Cardano:

  - Native multi-sig scripts

- General:

  - TSS (Threshold Signature Schemes): Used in Fireblocks, ZenGo, and
    other wallet infrastructures for keyless multi-party signatures.

  - MuSig2 (Bitcoin, Schnorr-based): Efficient, private, and scalable
    multi-sig protocol.

### Multi-Signature Accounts in Action

For enterprise grade management of multi-sig account some tools are
available like:

- Talisman Signet

- Multix from chainsafe

This section is mostly a video example of using Polkadot-JS for showing
multi-sigs in action.

In `polkadot-js` we can add addresses in our contacts (address book).
Then, we can select add a multi sig account and select the addresses for
this account from our address book. We set the threashold, and the name
for the group. The example shows the use of the multi-sig using Passeo
testnet , to simulate the use to sending some assets with a multi-sig
account.

### References

- <https://wiki.polkadot.network/learn/learn-cryptography/>

- <https://en.wikipedia.org/wiki/Digital_signature>

- <https://cqi.inf.usi.ch/blog/four.html>

- <https://polkadot-blockchain-academy.github.io/pba-content/singapore-2024/syllabus/1-Cryptography/6-Advanced_Signatures-slides.html#/4>

## Hash based Data Structures

### Data Structures for beginers

- What are data structures

Discuss few of them:

- Arrays

- LinkedLists

- Trees — max heap — binary search tree

each data structure has its own advatages and cons, depending on the use
case. we will see later that blockchains benifit from organizing data in
a specific structure

### Hash Chains

- hash based data structures (breafly compare with pointers if it is a
  good thing)

key feature: one-way functions we cant have cyclic structures with
hashes hashes only allow to reference previous data, never future data

- hash chains , explain it and its features

### Merkle Trees

- another key data structure in the context of block chains

- explain: binary tree where the value of each parent node is the hash
  of the child

- allows to prove the information about the tree without reveal the
  entire dataset

- allow to commit to the entire the entire dataset , by using the root
  hash we cannot change anu part of the tree without changing the root

- explain why it is benificial for blockchains

#### Merkle Example

- give an example of how to built a merkle tree and how to prove /
  verify values with it

### Efficient data structures

- briefly explain key value data bases

- explain a trie

- radix trie

- patricia tries

- explain MMR Merkle mountain range

 — also explain how to build and prove/verify — mention advantatges
regarding to traditional merkle trees.

### Summary / Take-away

- provie a summary take-away

### References

- <https://medium.com/@DevChy/introduction-to-data-structures-with-real-world-examples-15063e4adbad>

- <https://polkadot-blockchain-academy.github.io/pba-content/singapore-2024/syllabus/1-Cryptography/7-Hash_Based_Data_Structures-slides.html#/1>

## Advanced Cryptography Topics

### Advanced Cryptography Primitives

We have previously seen that multi-sig schemes can be usefull to avoid
single point of failures and loss of keys of one or more accounts in a
threshold multi-sig. One inconvenient side of using multi-sig for this
purpose is that we have to manage multiple accounts and their private
keys.

There is another technique that helps us to preserve a single private
key across multiple locations to avoid loss and failures. That is Shamir
secret sharing.

#### Shamir secret sharing

Shamir’s Secret Sharing (SSS) is a cryptographic method invented by Adi
Shamir in 1979. It allows a secret (like a password or cryptographic
key) to be split into multiple parts (shares). Only a minimum number of
those parts (a threshold) is needed to reconstruct the secret. Less than
that provides no information. This technique is similar to erasure
coding techniques.

It is based on polynomial interpolation over finite fields:

- Any k points uniquely determine a polynomial of degree k−1.

- Shares are points on a secret polynomial; the secret is the
  y-intercept.

**Benefits**

- High Security: No information is leaked unless the threshold is met.

- Redundancy: Shares can be distributed across multiple
  parties/locations to protect against data loss.

- Flexible Access Control: You can set custom thresholds (e.g., 3 out of
  5 shares).

**Limitations**

- Requires Secure Distribution: Shares must be securely delivered to
  participants.

- No Share Renewal: Shares can’t be easily changed without
  redistributing new ones.

- No Protection Against Collusion: If the threshold is met by colluding
  parties, the secret is fully revealed.

**Common Use Cases**

- Cryptocurrency Wallet Recovery: Splitting wallet keys among trusted
  parties.

- Key Management Systems: Secure backup of encryption keys.

- Nuclear Launch Codes: Distributed control among several
  decision-makers.

- Secure Multi-Party Agreements: Joint authorization requirements.

##### Quick comparison table: SSS vs Multi-sig

| Feature | SSS | Multi-Sig |
|----|----|----|
| **Concept** | Splits a single secret into parts | Multiple parties each have their own key |
| **Threshold** | Reconstruct secret with k of n shares | Requires k of n signatures to validate |
| **Trust Model** | Secret is fully revealed when threshold is met | Secret is never reconstructed; signatures prove agreement |
| **Risk** | Colluding threshold can expose the secret | Colluding threshold can approve transactions, but secret keys stay private |
| **Use Case** | Key backup, disaster recovery | Decentralized control (blockchains, wallets) |

### On-chain randomness - VRFs

Another interesting technique used in blockchains is VRF that is used to
obtain private randomness, that is publicly verifiable.

#### VRF: Verifiable Random Function

A Verifiable Random Function (VRF) is like a cryptographic “random
lottery” with proof. It produces a random-looking output that is: -
Unique: Deterministic for the same input. - Verifiable: Anyone can check
that the output is correct without knowing the secret key.

It’s similar to a cryptographic hash function, but with proof of
correctness tied to a private key. Introduced by Micali, Rabin, and
Vadhan in 1999, but gained significant use in blockchain protocols for
leader selection and randomness generation.

**Interface**

- eval(sk,input) → output

- sign(sk, input) → signature

- verify(pk, input, signature) → option output

The input can be a number that is known publicly and is agreed upon to
be the input by all the participants. By combining the input with the
secret key (sk), which is known only to the secret key owner, and
computing a hash of it results in an output that is pseudo-random. This
output can be revealed along with the signature later on, which can be
verified by anyone by using their public key (pk).

**Output properties**:

- Output is a deterministic function of key and input, i.e. eval should
  be deterministic

- It should be pseudo-random

- But until the VRF proof is revealed, only the holder of the secret key
  knows the output

- Revealing output does not leak secret key

**Usage**

Choose input after key, then the key holder cannot influence the output.
The output then is effectively a random number known only to the key
holder. But they can later reveal it, by publishing the VRF proof
(signature).

**Benefits**

- Unpredictability: Results can’t be predicted without the secret key.

- Verifiability: Others can independently verify the output using the
  public key.

- Fairness: Used to select participants randomly and fairly.

**Limitations**

- Requires Setup: Needs public/private key infrastructure.

- Deterministic: Given the same input and key, the output is always the
  same (can be a feature or a limitation).

- Complexity: More computationally intensive than simple random
  functions.

**Common Use Cases**

- Blockchain Leader Election: Example: Algorand uses VRFs to select
  block proposers randomly but verifiably.

- Random Number Generation: Secure lotteries, random selections where
  proof is required.

- Spam Prevention: Proof that a request is legitimate, used in anti-spam
  mechanisms.

**Example**

Players want to draw cards randomly and fairly without trusting each
other or a central dealer.

1.  Players Agree on a Random Input

    1.  All players agree on a shared random number x (this can come
        from a shared source like a blockchain block hash or a
        collectively agreed event).

    2.  This ensures no single player can choose the input to bias the
        result.

2.  Player A Draws a Card Using a VRF

    1.  Player A has a secret key (sk_A) and a corresponding public key.

    2.  A uses their VRF function to compute:
        `y = \text{eval}(sk_A, x) \mod 52`

        1.  `eval(sk_A, x)` : produces a pseudo-random but deterministic
            number tied to A’s secret key and input x.

        2.  mod 52 : maps the number to one of the 52 cards in a deck.

    3.  **Result**: Player A’s card is uniquely determined by their
        secret key and the shared input x. No one can predict A’s card
        in advance.

3.  Player A Publishes the VRF Proof

    1.  Along with the card, A publishes the VRF proof (a special
        signature).

    2.  This proof allows all other players to verify:

        1.  That A followed the correct process.

        2.  That A didn’t cheat or pick a different card.

            - The proof is verifiable using A’s public key and the
              shared input x.

            - Players can check the card without knowing A’s secret key.

#### VRF Types

Some important variations of VRFs are:

1.  Threshold VRF:

    A Threshold VRF allows a group of participants to jointly compute a
    VRF output without any single participant knowing the whole secret
    key.

    - The VRF secret key is split (e.g., using Shamir’s Secret Sharing).

    - A minimum number (threshold) of participants must cooperate to
      produce the VRF output and proof.

    - No single party controls the randomness.

      **Benefits**:

    - Decentralized and fault-tolerant.

    - Prevents bias or control by any single participant.

    - Used in decentralized randomness beacons (like in Dfinity’s
      consensus).

2.  Ring VRF

    A Ring VRF lets a participant generate a VRF output that is provably
    valid but anonymously linked to a group.

    - The proof shows the result was produced by someone in a predefined
      group but doesn’t reveal who.

    - Combines VRF with ring signatures (anonymous group signatures).

      **Benefits**:

    - Anonymity: Keeps the identity of the VRF generator hidden.

    - Verifiability: Others can still verify the result came from a
      valid group member.

    - Useful in privacy-preserving protocols, anonymous lotteries, or
      leader selection.

#### VRFs Take-away:

- **Fairness** comes from agreed input. No central dealer needed.

- **Randomness** comes from VRF evaluation. The combination of public
  input + secret key.

- **Security** comes from verifiable proof. A cannot lie about their
  draw.

- **Unpredictability** comes from the fact that results cannot be
  predicted without the secret key. Other players cannot predict or
  control the card A will draw.

### References

- <https://chain.link/education-hub/verifiable-random-function-vrf>

- <https://docs.polkadot.com/polkadot-protocol/basics/randomness/#vrf>

- <https://eprint.iacr.org/2023/002.pdf>

[^1]: We say “weak confidentiality” instead of “no confidentiality” to
    acknowledge that while digital signatures don’t provide real
    secrecy, they might, in very specific or indirect ways, limit
    exposure slightly. But it’s never enough to rely on for actual
    security of secret information.
