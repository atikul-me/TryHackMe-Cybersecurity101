# Hashing Basics - Comprehensive Notes

## 1. Introduction to Hash Functions

A **hash value** is a fixed-size string of characters or digest computed by a hash function. 
A **hash function** takes an input of an arbitrary size and returns an output of a fixed length. 

### Key Characteristics
*   **One-way:** It is computationally impractical to reverse the process (go from output back to input).
*   **No Key:** Unlike encryption, hashing does not require a key.
*   **Avalanche Effect:** Any slight change in the input data, even a single bit, causes a significant and unpredictable change in the output.
*   **Deterministic:** The same input will always produce the exact same hash value.

### Example of Avalanche Effect
Two files differing by a single bit (Letter 'T' = `01010100` vs Letter 'U' = `01010101`):
*   `T` -> MD5: `b9ece18c950afbfa6b0fdbfa4ff731d3`
*   `U` -> MD5: `4c614360da93c0a041b22e537de151eb`
*(Notice the completely different hash outputs despite only a 1-bit change.)*

### Hash Collisions
A **hash collision** occurs when two different inputs produce the exact same output. 
*   **Pigeonhole Effect:** Since inputs are practically unlimited but outputs are limited to a fixed number of bits, collisions are theoretically unavoidable. 
*   **Algorithm Security:** Good hash functions make collisions extremely rare and difficult to engineer. **MD5** and **SHA-1** have been broken and are considered insecure due to engineered collisions. Do not use them for passwords or data integrity.

---

## 2. Hashing for Password Storage

Storing passwords in plaintext, using deprecated encryption, or using insecure hash functions (like unsalted SHA-1) are major security risks. 

### The Problem with Plain Hashing
If a database is leaked, attackers can use **Rainbow Tables** (precomputed lookup tables of hashes to plaintexts) to instantly crack unsalted hashes. Furthermore, if two users have the same password, they will have the same hash.

### The Solution: Salting
A **salt** is a unique, randomly generated value added to the password before hashing (e.g., `Hash(Password + Salt)`). 
*   Salts do not need to be kept private and are stored alongside the hash.
*   Salting ensures that identical passwords yield completely different hashes, rendering Rainbow Tables useless.
*   **Secure Algorithms:** Argon2, Scrypt, Bcrypt, and PBKDF2 automatically handle salting. They are designed to be computationally expensive, making brute-force and GPU attacks much slower.

> **Why not Encryption?** Encryption is reversible and requires a key. If the key is compromised, all passwords are instantly exposed. Hashing is one-way, making it the correct tool for authentication.

---

## 3. Hashing for Data Integrity & Authentication

### Integrity Checking
Hashing is used to verify that files have not been altered. You can compare the hash of a downloaded file to the official hash provided by the source (e.g., Fedora Workstation ISO SHA256 checksums). If the hashes match, the file is identical bit-for-bit.

### HMAC (Keyed-Hash Message Authentication Code)
HMAC uses a cryptographic hash function combined with a **secret key** to verify both *authenticity* and *integrity*.
*   **Formula:** `HMAC(K, M) = H((K ⊕ opad) || H((K ⊕ ipad) || M))`
*   **ipad (inner pad):** Byte `0x36` repeated to block size.
*   **opad (outer pad):** Byte `0x5C` repeated to block size.
*   The use of the secret key ensures that only someone possessing the key can generate the HMAC, proving the message's origin and that it hasn't been tampered with.

---

## 4. Hash Identification & Cracking (Offensive Security)

### Identifying Hashes
Automated tools like `hashID` exist, but context is crucial. (e.g., 32 hex characters is usually MD5; a string starting with `$2a$` is Bcrypt).

**Linux Password Storage (`/etc/shadow`):**
Format: `$prefix$options$salt$hash`

| Prefix | Algorithm | Description |
| :--- | :--- | :--- |
| `$y$` | yescrypt | Default/recommended on newer Linux systems |
| `$gy$` | gost-yescrypt | Uses GOST R 34.11-2012 |
| `$7$` | scrypt | Password-based key derivation function |
| `$2b$`, `$2y$`, `$2a$`, `$2x$` | bcrypt | Based on Blowfish block cipher |
| `$6$` | sha512crypt | Based on SHA-2 with 512-bit output (older Linux) |
| `$1$` | md5crypt | Based on MD5 (insecure) |

*   **Windows:** Hashes stored in the SAM (Security Accounts Manager) using NTLM (variant of MD4). Visually identical to MD4/MD5. Tools like `mimikatz` can dump them.
*   **macOS:** Stores passwords as salted PBKDF2 hashes (SHA-512) in `/var/db/dslocal/nodes/Default/users/`.

### Cracking Hashes
You cannot "decrypt" hashes; you must hash candidate passwords (like those in `rockyou.txt`) and compare them to the target hash. GPUs (Graphics Processing Units) are highly efficient at this due to their thousands of cores.

**Hashcat Basic Syntax:**
```bash
hashcat -m <hash_type> -a <attack_mode> hashfile wordlist
