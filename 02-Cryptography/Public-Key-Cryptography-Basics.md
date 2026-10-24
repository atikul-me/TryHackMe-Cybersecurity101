# Public Key Cryptography Basics

This room is the second of three introductory rooms about cryptography. Before starting this room, ensure you have finished the first one on the list:
- Cryptography Basics
- Public Key Cryptography Basics (this room)
- Hashing Basics

---

## 1. Introduction: The Security Pillars
To understand the need for cryptography, let's compare an in-person meeting with correspondence in the cyber realm:

* **Authentication:** Confirming the identity of who you are talking with.
* **Authenticity:** Verifying that the message genuinely comes from a specific sender.
* **Integrity:** Ensuring that the data has not been altered, tampered with, or changed during transmission.
* **Confidentiality:** Ensuring that only authorised parties can access the data (preventing unauthorized eavesdropping).

Private key cryptography (symmetric encryption) mainly protects confidentiality. However, **public key cryptography (asymmetric cryptography)** plays a significant role in authentication, authenticity, and integrity.

---

## 2. Analogy for Asymmetric Cryptography
* **Secret Code:** Symmetric encryption cipher and key.
* **Lock:** Server's public key.
* **Lock's Key (Opening Key):** Server's private key.

Asymmetric cryptography is relatively slow compared to symmetric encryption; therefore, we rely on asymmetric encryption once to securely negotiate and agree on symmetric encryption ciphers and keys, and then communicate privately using symmetric encryption.

---

## 3. RSA (Rivest–Shamir–Adleman)
RSA is a public-key encryption algorithm that enables secure data transmission over insecure channels. 

### The Math That Makes RSA Secure
* Based on the mathematically difficult problem of factoring a large number. Multiplying two large prime numbers is straightforward, but finding the prime factors of a huge number takes immense computing power.
* While computers can easily factor numbers, they cannot efficiently factorize a number with more than 600 digits when chosen as the product of two ~300-digit prime numbers.

### Simplified Numerical Example
1. Bob chooses two prime numbers: $p = 157$ and $q = 199$. He calculates $n = p \times q = 31243$.
2. With $\phi(n) = n - p - q + 1 = 31243 - 157 - 199 + 1 = 30888$, Bob selects $e = 163$ such that $e$ is relatively prime to $\phi(n)$.
3. He selects $d = 379$, where $e \times d = 1 \pmod{\phi(n)}$, i.e., $163 \times 379 = 61777$ and $61777 \pmod{30888} = 1$.
4. **Public Key:** $(n, e) = (31243, 163)$
5. **Private Key:** $(n, d) = (31243, 379)$
6. **Encryption:** Alice calculates $y = x^e \pmod{n} = 13^{163} \pmod{31243} = 16341$.
7. **Decryption:** Bob decrypts by calculating $x = y^d \pmod{n} = 16341^{379} \pmod{31243} = 13$.

### RSA in CTFs
* **Variables:** $p$, $q$ (large primes), $n$ ($p \times q$), $e$ (public exponent), $d$ (private exponent), $m$ (plaintext message), $c$ (ciphertext).
* **Tools:** `RsaCtfTool`, `rsatool`.

---

## 4. Diffie-Hellman Key Exchange
Key exchange aims to establish a shared secret between two parties over an insecure channel without requiring a pre-existing shared secret.

### Process:
1. **Public Variables:** Agree on a large prime number $p$ and a generator $g$ ($0 < g < p$).
2. **Private Integers:** Alice chooses private key $a$, and Bob chooses private key $b$.
3. **Public Keys:** 
   - Alice calculates $A = g^a \pmod{p}$
   - Bob calculates $B = g^b \pmod{p}$
4. **Key Exchange:** They exchange public keys ($A$ and $B$).
5. **Shared Secret:** 
   - Alice calculates $B^a \pmod{p}$
   - Bob calculates $A^b \pmod{p}$
   - Both yield the exact same shared secret: $g^{ab} \pmod{p}$.

---

## 5. Authenticating the Server and Client (SSH)
* **Server Authentication:** Checked via public key fingerprint (e.g., ED25519) to prevent Man-in-the-Middle (MitM) attacks.
* **Client Authentication:** Using SSH keys instead of passwords.
  - Generated using `ssh-keygen -t ed25519`.
  - **Public Key (`id_ed25519.pub`):** Placed in the remote host's `~/.ssh/authorized_keys` file.
  - **Private Key (`id_ed25519`):** Kept strictly secure and treated like a password (permissions must be `600` or stricter).

---

## 6. Digital Signatures and Certificates
* **Digital Signature:** Provides authenticity and integrity of a digital message. Produced using a private key and verified using the corresponding public key.
* **Certificates (HTTPS/TLS):** Prove the identity of a server via a **Chain of Trust** starting from a Root Certificate Authority (CA) trusted by your operating system and web browser.

---

## 7. PGP (Pretty Good Privacy) and GPG
* Open-source implementation: **GnuPG (GPG)**.
* Used to encrypt files, sign emails, and confirm integrity.
* **Key Generation:** `gpg --full-gen-key` (often using ECC or RSA).
* **Importing/Decrypting:**
  - Import key: `gpg --import backup.key`
  - Decrypt message: `gpg --decrypt confidential_message.gpg`

---

## 8. Key Terminology
* **Cryptography:** The science of securing communication and data using codes and ciphers.
* **Cryptanalysis:** The study of methods to break or bypass cryptographic security systems without knowing the key.
* **Brute-Force Attack:** Trying every possible key or password combination.
* **Dictionary Attack:** Trying dictionary words or combinations of them.
