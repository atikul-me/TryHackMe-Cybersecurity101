# Cryptography Basics - Room Notes

This document contains key notes, summaries, and mathematical foundations from the TryHackMe introductory room on Cryptography Basics.

---

## 1. Introduction to Cryptography

Cryptography lays the foundation for our digital world. While networking protocols enable devices across the globe to communicate, cryptography makes it possible to trust this communication.

- **Ultimate Purpose:** To ensure secure communication in the presence of adversaries and third parties.
- **Core Security Goals (CIA Triad):**
  - **Confidentiality:** Preventing unauthorized disclosure of messages.
  - **Integrity:** Ensuring data is not altered or tampered with.
  - **Authenticity / Verification:** Being confident in connecting with the real server/source.

### Common Real-World Scenarios:
- **Credentials Protection:** When logging into TryHackMe, credentials are encrypted during transmission.
- **Secure Shell (SSH):** Establishing an encrypted tunnel to prevent eavesdropping.
- **Online Banking:** Browsers check remote server certificates to prevent man-in-the-middle attacks.
- **File Integrity:** Using hash functions to confirm downloaded files are identical to the originals.

---

## 2. Compliance Standards & Regulations

Handling sensitive data requires strict compliance with international and national standards:

- **PCI DSS (Payment Card Industry Data Security Standard):** 
  - Ensures minimum security levels to store, process, and transmit credit card information.
  - Requires data encryption both **at rest** (stored) and **in motion** (transmitted).
- **Medical & Healthcare Records Regulations:**
  - **HIPAA & HITECH:** United States.
  - **GDPR:** European Union (EU).
  - **DPA:** United Kingdom (UK).

---

## 3. Historical Ciphers

Before modern computational cryptography, historical ciphers relied on simple substitution or transposition techniques.

- **Caesar Cipher:**
  - One of the simplest and oldest substitution ciphers.
  - It works by shifting each letter in the plaintext by a fixed number of positions down the alphabet (e.g., with a shift of 3, 'A' becomes 'D', 'B' becomes 'E', etc.).
  - **Vulnerability:** Extremely weak against modern cryptanalysis because there are only 25 possible shifts in the English alphabet, making it vulnerable to Brute-Force attacks easily.

---

## 4. Symmetric Encryption Standards

Symmetric encryption uses the **same key** for both encryption and decryption.

- **DES (Data Encryption Standard):**
  - Adopted in 1977.
  - Uses a 56-bit key (successfully broken in under 24 hours back in 1999).
- **3DES (Triple DES):**
  - Applied DES three times (key size is 168 bits, effective security is 112 bits).
  - Acted as an ad-hoc solution when DES became insecure. Deprecated in 2019.
- **AES (Advanced Encryption Standard):**
  - Adopted as a standard in 2001.
  - Key sizes can be **128, 192, or 256 bits**.

---

## 5. Asymmetric Encryption (Public Key Cryptography)

Unlike symmetric encryption, asymmetric encryption uses a **pair of keys**: a public key to encrypt and a private key to decrypt.

- **Key Types:**
  - **Public Key:** Shared with everyone. Used to protect confidentiality.
  - **Private Key:** Kept secret and guarded by the owner.
- **Examples & Key Sizes:**
  - **RSA:** Uses 2048-bit, 3072-bit, and 4096-bit keys (2048-bit is recommended minimum).
  - **Diffie-Hellman:** Recommended minimum key size of 2048 bits (also uses 3072-bit and 4096-bit).
  - **ECC (Elliptic Curve Cryptography):** Achieves equivalent security with shorter keys (e.g., a 256-bit ECC key matches a 3072-bit RSA key).
- **Mathematical Foundation:** Based on problems that are easy to compute in one direction but practically infeasible/extremely difficult to reverse.

---

## 6. Mathematical Building Blocks

Modern cryptography relies heavily on fundamental mathematical operations:

### 1. XOR Operation (Exclusive OR)
- **Concept:** A bitwise logical operation that compares two bits. If the bits are different, the output is `1`. If the bits are the same, the output is `0`.
- **Truth Table:**
  - `0 ⊕ 0 = 0`
  - `0 ⊕ 1 = 1`
  - `1 ⊕ 0 = 1`
  - `1 ⊕ 1 = 0`
- **Reversibility:** XOR is symmetric/reversible. If you encrypt data by XORing it with a key ($Text \oplus Key = Cipher$), you can decrypt it by applying the exact same key to the cipher ($Cipher \oplus Key = Text$). This makes it fundamental in cryptography streams.

### 2. Modulo Operation ($\text{mod}$)
- **Concept:** The modulo operation finds the remainder after division of one number by another. For example, $17 \pmod 5 = 2$ because $17 \div 5 = 3$ with a remainder of $2$.
- **Role in Cryptography:** 
  - It creates a **cyclic/finite range** of numbers (wrapping around after a certain value, like a clock wrapping around after 12 hours).
  - Used extensively in symmetric and asymmetric algorithms (like RSA and Diffie-Hellman) to keep numbers within manageable limits and ensure operations remain one-way functions.
