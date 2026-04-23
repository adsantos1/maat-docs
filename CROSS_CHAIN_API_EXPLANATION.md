# The Maat-Auth Cross-Chain API: A Non-Technical Guide

This document explains how the Cross-Chain API (CC-API) transforms Maat from a closed network into a **global infrastructure** for trust and verification.

---

### 1. What is it meant for? (The "Why")

Imagine you have an Ethereum Smart Contract that automatically pays a supplier $10,000 once a "Bill of Lading" (shipping document) is signed. 
*   **The Problem:** Ethereum is terrible at storing and verifying heavy, real-world documents. It's too expensive, too slow, and not built for privacy.
*   **The Solution:** The supplier authenticates the Bill of Lading on **Maat** (because Maat is built specifically for secure, low-cost document and identity verification). Then, the Ethereum contract asks the **Maat CC-API**, *"Hey, is this document legit?"* 

The CC-API is the bridge that allows a massive financial network (like Ethereum or Solana) to securely trust the identity and document verification done on Maat. It turns Maat into the **"Truth Layer"** for all of Web3.

### 2. How does it work technically? (The "How")

It is a simple, standardized REST API endpoint running directly on your Hetzner L1 node. 
We built two specific URLs for this purpose:
1.  **`/api/v1/cross-chain/verify-document/<document_hash>`**
2.  **`/api/v1/cross-chain/verify-identity/<did_profile>`**

When an external system calls one of these URLs, the Maat node doesn't just say "Yes." It searches your PostgreSQL database, finds the exact block where that document or identity was verified, and generates a **"Truth Certificate."**

This certificate is a clean JSON package that proves without a doubt:
*   The exact time the data was anchored.
*   The exact block number it resides in.
*   The validator who confirmed it.
*   The wallet that owns it.

### 3. How do people use it? (The Developer Experience)

It is designed to be incredibly easy for any developer (Web2 or Web3) to use. They don't need a Maat wallet or complicated blockchain knowledge just to *read* the truth.

**Example Scenario: A Bank Verifying a User's ID**
1.  A user applies for a loan on a traditional Web2 banking site.
2.  The user provides their Maat DID (e.g., `did:maat:12345`).
3.  The bank's backend server sends a simple HTTP GET request to your Hetzner node:
    `GET http://204.168.128.54:8546/api/v1/cross-chain/verify-identity/did:maat:12345`
4.  Your node instantly replies with the "Truth Certificate":
    ```json
    {
      "verified": true,
      "identity": {
        "name": "John Doe",
        "trust_score": "12.5",
        "is_verified": true
      }
    }
    ```
5.  The bank trusts the response because it came directly from the immutable Maat Layer 1 node, and they approve the loan.

### The Future: Programmable Oracles & Monetization

Right now, this API acts as a free public verifier. But in the next phase (**Programmable Oracles**), we will build logic so that every time a massive entity (like an Ethereum contract or a Bank) makes one of these API calls, they have to pay a tiny "Trust Fee" in Meri. 

That is how the network generates massive, sustainable real-world revenue: by becoming the toll booth for global digital truth.
