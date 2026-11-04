# Hashing Basics

## Introduction to Hash Functions

Consider the scenario where you just downloaded a 6 GB file and want to know whether the copy you downloaded is identical to the original file, bit for bit. Or if a good Samaritan handed you this 6 GB file on a USB memory drive, how can you be sure it is identical to the file you want to download?

The answer to both of the above questions lies in comparing the hash values of the two files; if two hash values are equal, you can say with very high certainty that the two files are identical. 

A **hash value** is a fixed-size string of characters or digest that is computed by a hash function. A **hash function** takes an input of an arbitrary size and returns an output of fixed length.

*Note on terminology: We favour the terms hash function and hash value. However, we occasionally use the word hash as a verb to mean calculate the hash value; moreover, we occasionally use the word hash by itself as a noun to refer to the hash value.*

### Learning Objectives
*   Hash functions and collisions
*   The role of hashing in authentication systems
*   Recognizing stored hash values
*   Cracking hash values
*   The use of hashing for integrity protection

---

## What is a Hash Function?

Hash functions are different from encryption. There is no key, and it’s meant to be impossible (or computationally impractical) to go from the output back to the input.

A hash function takes some input data of any size and creates a summary or digest of that data. The output has a fixed size. It’s hard to predict the output for any input and vice versa. Good hashing algorithms will be relatively fast to compute and prohibitively slow to reverse. Any slight change in the input data, even a single bit, should cause a significant change in the output.

### The Avalanche Effect
Consider two files: `file1.txt` contains the letter **T**, while `file2.txt` contains the letter **U**. 
*   The letter T is `54` in hexadecimal, i.e., `01010100` in binary.
*   The letter U is `55` in hexadecimal, i.e., `01010101` in binary.

The files differ by a single bit. However, if we compare their hashes, they are entirely different:

```bash
strategos@g5000 ~> cat file1.txt 
T⏎
strategos@g5000 ~> cat file2.txt 
U⏎   

strategos@g5000 ~> md5sum *.txt
b9ece18c950afbfa6b0fdbfa4ff731d3  file1.txt
4c614360da93c0a041b22e537de151eb  file2.txt

strategos@g5000 ~> sha1sum *.txt
c2c53d66948214258a26ca9ca845d7ac0c17f8e7  file1.txt
b2c7c0caa10a0cca5ea7d69e54018ae0c0389dd6  file2.txt

strategos@g5000 ~> sha256sum *.txt
e632b7095b0bf32c260fa4c539e9fd7b852d0de454e9be26f24d0d6f91d069d3  file1.txt
a25513c7e0f6eaa80a3337ee18081b9e2ed09e00af8531c8f7bb2542764027e7  file2.txt
