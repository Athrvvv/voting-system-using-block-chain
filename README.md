# Voting System Using Blockchain

## Table of Contents

1. [Project Overview](#project-overview)
2. [Blockchain Concepts](#blockchain-concepts)
3. [Project Flow](#project-flow)
4. [Blockchain Implementation in This Project](#blockchain-implementation-in-this-project)
5. [Features](#features)
6. [Setup & Execution](#setup--execution)
7. [Dependencies](#dependencies)
8. [Screenshots](#screenshots)
9. [Future Enhancements](#future-enhancements)

---

## Project Overview

This project implements a secure **voting system using blockchain technology**. The system allows voters to authenticate using their Aadhar number, verify their identity via email OTP, and securely cast votes. Each vote is cryptographically signed using ECC (Elliptic Curve Cryptography) and stored in a blockchain-like structure ensuring immutability and tamper-resistance.

### Purpose

* Ensure transparency and security in digital voting.
* Prevent double voting and tampering.
* Maintain a verifiable and auditable voting record.

---

## Blockchain Concepts

Blockchain is a distributed ledger technology where data is stored in blocks that are cryptographically linked:

1. **Blocks**: Contain a set of transactions (votes in our case), a timestamp, a nonce, and the previous block's hash.
2. **Merkle Tree**: Efficiently verifies integrity of multiple transactions by storing only the Merkle root in the block.
3. **Proof-of-Work (Mining)**: Ensures blocks are validated via computational work (hashes with leading zeros).
4. **Hashing**: SHA3-256 is used to hash transactions and block contents, ensuring immutability.
5. **ECC Signatures**: Each vote is signed using ECC to guarantee authenticity and non-repudiation.

In this project, these concepts are combined to create a secure voting workflow.

---

## Project Flow

1. **Voter Authentication**

   * Voter enters Aadhar number.
   * System checks validity and whether the voter has already voted.

2. **Email Verification**

   * OTP is sent to the voter's registered email.
   * Voter enters OTP to verify their identity.

3. **Key Generation**

   * A unique ECC private-public key pair is generated.
   * Private key is sent via email; public key stored on the server.

4. **Voting**

   * Voter selects a political party.
   * Vote is signed using private key and verified using public key.

5. **Vote Storage**

   * Vote is stored in a `Vote` table.
   * Backup votes stored in `VoteBackup` table.
   * Votes are grouped into blocks for blockchain storage.

6. **Mining Blocks**

   * Non-sealed votes are grouped based on transactions per block.
   * Merkle root is generated.
   * Proof-of-work puzzle solved to seal the block.
   * Block saved with nonce, timestamp, Merkle root, and previous hash.

7. **Result Verification**

   * Votes can be counted.
   * Blockchain tampering can be detected by recalculating Merkle roots.

---

## Blockchain Implementation in This Project

1. **Block Structure**

   * `block_id` (unique block number)
   * `prev_hash` (hash of previous block)
   * `merkle_hash` (Merkle root of votes in block)
   * `this_hash` (hash of current block with nonce)
   * `nonce` (Proof-of-Work)
   * `timestamp`

2. **Vote Storage**

   * Votes stored with UUID, party selection, timestamp.
   * Backup table ensures integrity and recovery.

3. **Merkle Tree**

   * Each vote string concatenation hashed.
   * Leaves added to Merkle tree.
   * Root stored in block.

4. **Proof-of-Work**

   * Hash puzzle solved with leading zeros (`PUZZLE` setting).
   * Ensures computational effort before block addition.

5. **ECC Signatures**

   * Votes are signed using ECC private key.
   * Public key used to verify vote authenticity.

---

## Features

* Secure voter authentication using Aadhar number.
* Email OTP verification.
* Private key generation and delivery.
* ECC-based vote signing.
* Blockchain storage of votes with Proof-of-Work.
* Merkle tree verification.
* Tamper detection.
* Admin view for mining and blockchain verification.
* Vote counting and display.

---

## Setup & Execution

### 1. Clone the repository

```bash
git clone <repository-url>
cd voting-system-using-block-chain
```

### 2. Create virtual environment

```bash
python -m venv venv
source venv/bin/activate  # Linux/Mac
venv\Scripts\activate     # Windows
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure settings

* Set `EMAIL_ADDRESS` and `EMAIL_PASSWORD` in `settings.py` (use Gmail App Password).
* Optional: `TRANSACTIONS_PER_BLOCK`, `PUZZLE`, `PLENGTH`.

### 5. Migrate database

```bash
python manage.py makemigrations
python manage.py migrate
```

### 6. Run server

```bash
python manage.py runserver
```

### 7. Access Application

* Open `http://127.0.0.1:8000/`
* Authenticate, verify email, cast vote, and explore blockchain.

---

## Dependencies

* Python 3.10+
* Django 5.2
* pycryptodome (for ECC & SHA3)
* smtplib (for email)
* jQuery (frontend)
* Bootstrap/TailwindCSS for UI (optional)

---

## Screenshots

*(Add relevant screenshots here if needed)*

---

## Future Enhancements

* Multi-factor authentication.
* Improved UI/UX for vote casting.
* Real-time blockchain visualization.
* Admin panel for election setup.
* Integration with real Aadhar verification APIs.
* Mobile responsiveness and PWA support.

---

**Note:** This project is a demonstration of blockchain-based secure voting and **should not be used for real elections** without additional security audits.
