# TryHackMe: Darkmatter — Writeup & Walkthrough

A concise technical walkthrough for cracking the **Darkmatter** challenge on TryHackMe by extracting RSA public key parameters and deriving the private exponent to decrypt target files.

---

## 📌 Challenge Summary

* **Platform:** TryHackMe  
* **Room:** Darkmatter  
* **Focus Area:** Cryptography, RSA Key Analysis, System Artifact Inspection

---

## 🕵️‍♂️ Step-by-Step Solution

### Step 1: Discovering Public Key Artifacts

During system enumeration, inspect the temporary directory for dropped files or configuration artifacts:

```sh
cd /tmp
ls -la
```

Located the artifact `public_key.txt`. Inspecting its contents revealed the target RSA public key parameters:

* Public Exponent ($e$): `65537`  
* Modulus ($n$): Expose/extract the full decimal integer value of $n$ from `public_key.txt`.

### Step 2: Deriving the Private Key Exponent ($d$)

Since the modulus $n$ is factorable into prime factors $p$ and $q$ ($n=p\times q$), compute the Euler totient function $\phi (n)=(p-1)(q-1)$ to find the modular multiplicative inverse:

$$
d \equiv e^{-1} \pmod{\phi(n)}
$$

&nbsp;

1. Extract $n$ from `public_key.txt`.  
2. Input $n$ and $e=65537$ into an online RSA calculator / solver tool (e.g., dCode, Factordb, or `factordb-py`).  
3. Factorize $n$ into $p$ and $q$ to calculate the private exponent **$d$**.

### Step 3: Decrypting Target Files

1. Copy the calculated value of **$d$**.  
2. Return to the virtual Linux lab machine.  
3. Pass the derived exponent $d$ into the target decryption script / input box to perform the modular exponentiation for decryption:

$$
m \equiv c^d \pmod n
$$

&nbsp;

Both encrypted target files successfully decrypted into readable text format.

### Step 4: Flag Extraction

Inspect the contents of the newly decrypted files to retrieve the hidden flag:

```
THM{...}
```

---

## 🛡️ Key Takeaways

* **Weak RSA Keys:** Small or improperly generated modulus values ($n$) allow attackers to quickly factorize $n\rightarrow (p,q)$ and derive $d$.  
* **Insecure Storage:** Storing cryptographic keys or parameters in world-readable directories like `/tmp` allows unprivileged users to audit and reconstruct keys.

---

*Documented for cybersecurity research and lab documentation.*