# 🔐 b00tl3gRSA2
**PicoCTF Difficulty:** Hard

## **Description**  

```
In RSA d is a lot bigger than e, why don't we use d to encrypt instead of e?
```

## **Explanation**
This challenge uses RSA encryption, however, once the public exponent $e$ and private exponent $d$ are calculated, their respective variables are swapped.

We are given the ciphertext $c$, the modulus $n$, and a value labeled $e$. Because the variables $e$ and $d$ were swapped before being published, this value provided for $e$ is actually $d$.

In RSA encryption, $d$ is used as the private exponent for decryption as it is calculated by:

$$d = e^{-1} \mod \phi (n) $$

Requiring Euler's Totient $\phi (n)$, which can only be calculated using the secret prime numbers $p$ and $q$:

$$\phi (n) = (p - 1)(q - 1)$$

On the other hand, the public exponent $e$ is a number chosen that satisfies:

$$1 < e < \phi (n) \land \gcd(e, \phi (n)) = 1$$

In the real world application of RSA, the public exponent $e$ is often chosen as $65537$. This number is known as a Fermat Prime, specifically $F_4$, the largest Fermat Prime found. Fermat Numbers are expressed in the form:

$$F_n = 2^{2^n} + 1$$

Where:

$$ 65537 = 2^{2^4} + 1$$

This specific number $65537$ is chosen primarily because of its extremely fast computational speed. Since its binary representation is expressed as:

$$10000000000000001$$

This binary number has a [Hamming Weight](https://en.wikipedia.org/wiki/Hamming_weight) of $2$, which allows the computer to calculate exponentiation through only $16$ squares and one multiplication:

$$c = m^{65537} \mod n = m^{{2^{16}} + 1} \mod n$$

Additionally, $65537$ is large enough to be secure from low exponent attacks such as Håstad's Broadcast Attack, or Coppersmith's Attack.

Since we know that the number $65537$ is often used as the public exponent $e$ for RSA, and that the values for $e$ and $d$ were switched, we can decrypt the ciphertext $c$ using a private exponent value of $65537$:

$$m = c^{65537} \mod n$$
  
## **Code / Commands / Images**
```Python
c = [REDACTED]

n = [REDACTED]

e = [REDACTED]

m = pow(c, 65537, n)

plaintext_bytes = m.to_bytes(
    (m.bit_length() + 7) // 8, byteorder='big')

plaintext = plaintext_bytes.decode("utf-8")

print(plaintext)

```

[🏠 Back to Main Page](https://github.com/Greenest-Guy/RSA-CTF-Writeups)
