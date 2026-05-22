# 🔐 ClusterRSA
**Difficulty:** Medium

**Files/Links Provided:** ```message.txt```

## **Description**  
```
A message has been encrypted using RSA, but this time something feels... more crowded than usual. Can you decrypt it?
Download the message.
```

## **Explanation**
This challenge presents an RSA modulus built from more than 2 unique prime factors. The security of the RSA cryptosystem relies on the difficulty of [integer factorization](https://en.wikipedia.org/wiki/Integer_factorization), specifically, the factorization of a modulus build off of 2 sufficiently large primes (generally 1024 bits each, creating a 2048-bit modulus).

The vulnerability here is that the modulus is constructed from multiple smaller primes rather than two large ones. For a fixed-size modulus, using more primes means each individual prime is proportionally smaller. This matters because various prime decomposition algorithms time complexities are based upon the size of the factors not the size of the modulus, therefor smaller factors make factorization dramatically faster.

Being aware of this vulnerability allows us to [decompose the prime factors](https://www.dcode.fr/prime-factors-decomposition) that the modulus was built upon, and reconstruct the private key.

To calculate the private exponent using multiple prime numbers we must first calculate Euler's Totient with $k$ prime numbers:

$$\phi(n) = \prod_{i=1}^{k} (p_i - 1)$$

Where in this case with 4 primes:

$$\phi (n) = (p_1 - 1)(p_2 - 1)(p_3 - 1)(p_4 - 1)$$

Now having the public exponent $e$ and Euler's Totient we can calculate the private exponent $d$ as follows:

$$d \equiv e^{-1} \pmod{\phi(n)}$$

Finally, the ciphertext $c$ can be decrypted:

$$m = c^d \mod (n)$$

## **Code / Commands / Images**

[```Prime Factor Decomposition```](https://www.dcode.fr/prime-factors-decomposition)

```Python
def decrypt(ciphertext, private_exponent, modulus) -> tuple:
    m = pow(ciphertext, private_exponent, modulus)

    plaintext_bytes = m.to_bytes(
        (m.bit_length() + 7) // 8, byteorder='big')

    try:
        plaintext = plaintext_bytes.decode('utf-8')
        utf8 = True

    except UnicodeDecodeError:
        plaintext = plaintext_bytes.decode('latin-1')
        utf8 = False

    return plaintext, utf8


ct = CIPHERTEXT

n = MODULUS

e = PUBLIC_EXPONENT

if __name__ == '__main__':
    # Prime Factor Decomposition of n
    factors = [factor1, factor2,
               factor3, factor4]

    # Calculate eulers totient using 4 prime factors
    eulers_totient = (factors[0] - 1) * (factors[1] -
                                         1) * (factors[2] - 1) * (factors[3] - 1)

    # Calculate private exponent
    d = pow(e, -1, eulers_totient)

    # Decrypt cipher text
    print(decrypt(ct, d, n))

```
> 

[🏠 Back to Main Page](https://github.com/Greenest-Guy/RSA-CTF-Writeups)
