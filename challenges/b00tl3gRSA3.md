# 🔐 b00tl3gRSA3

**PicoCTF Difficulty:** Hard

## **Description**  

```
Why use p and q when I can use more?
```

## **Explanation**

This challenge presents an RSA modulus built from more than 2 unique prime factors. The security of the RSA cryptosystem relies on the difficulty of [integer factorization](https://en.wikipedia.org/wiki/Integer_factorization), specifically, the factorization of a modulus built off of 2 sufficiently large primes (generally 1024 bits each, creating a 2048-bit modulus).

The vulnerability occurs due to the fact that the modulus is constructed from multiple smaller primes rather than two large ones. For a fixed-size modulus, using a greater number of primes correlates to each individual prime being proportionally smaller. This matters because various prime decomposition algorithms time complexities are based upon the size of the factors as opposed to the size of the modulus, therefore smaller factors make factorization dramatically faster.

Being aware of this vulnerability allows us to [decompose the prime factors](https://www.dcode.fr/prime-factors-decomposition) that the modulus was built upon, and reconstruct the private key.

To calculate the private exponent using multiple prime numbers we must first calculate Euler's Totient with $k$ prime numbers:

$$\phi(n) = \prod_{i=1}^{k} (p_i - 1)$$

Where in this case we have 34 prime numbers, making $k = 34$

Now having the public exponent $e$ and Euler's Totient we can calculate the private exponent $d$ as follows:

$$d = e^{-1} \mod \phi (n) $$

Finally, the ciphertext $c$ can be decrypted:

$$m = c^d \mod (n)$$

## **Code / Commands / Images**
```Python
import math

n = [REDACTED]
c = [REDACTED]
e = 65537

factor_str = (
    "8628384487 × 8983941913 × 9013557073 × 9110237153 × 9194246027 × "
    "9682016213 × 9956051093 × 10312803421 × 10610709503 × 11149237501 × "
    "11326443389 × 11401200241 × 11480003551 × 11533287749 × 12261684173 × "
    "12632610667 × 12685319597 × 12725482873 × 12900984121 × 13596426947 × "
    "13955871451 × 14297317103 × 14448258613 × 14626816483 × 15162210821 × "
    "15418090547 × 15886604759 × 15914333923 × 15981765571 × 16315428127 × "
    "16481380933 × 16482285089 × 16618367971 × 16939810637"
)

# Factors decomposed using https://www.dcode.fr/prime-factors-decomposition

factors = [int(f.strip()) for f in factor_str.split("×")]
phi_n = math.prod(p - 1 for p in factors)

d = pow(e, -1, phi_n)
m = pow(c, d, n)

plaintext = bytes.fromhex(f"{m:x}").decode("utf-8")

print(plaintext)

```
> 

[🏠 Back to Main Page](https://github.com/Greenest-Guy/RSA-CTF-Writeups)
