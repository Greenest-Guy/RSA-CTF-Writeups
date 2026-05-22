# 🔐 EVEN RSA CAN BE BROKEN???
**Difficulty:** Easy

## **Description**  

```
This service provides you an encrypted flag. Can you decrypt it with just N & e?
```

## **Steps to Solve**  
1. Use netcat to connect to the program.
2. Extract $N$, $e$, and $c$ for analysis.
3. Since the modulus $n$ is an even number, 2 can be extracted as a prime factor.
4. Reconstruct the private key $d$ and decrypt the message revealing the flag.

## **Explanation**
The security of RSA is dependent on the difficulty of factoring the modulus $n$. If the modulus $n$ is an even number, the encryption completely breaks due to the fact that 2 can be easily discovered as a prime factor.
This makes it so that the private exponent $d$ can be reconstructed.

Since we know one of the prime factors $p = 2$, we can calculate the second prime factor $q$ as simply:

$$q = n/2$$

Euler's Totient can then be calculated as:

$$\phi (n) = (p - 1)(q - 1) \implies \frac{n}{2} - 1$$

Allowing us to calculate the private exponent $d$ as follows:

$$d = e^{-1} \mod \phi (n) $$

Finally the message can be decrypted:

$$m = c^d \mod n$$

## **Code / Commands / Images**
```Python
c = [REDACTED]
n = [REDACTED]
e = 65537

p = 2

q = n // 2

phi_n = (p - 1) * (q - 1)

d = pow(e, -1, phi_n)

m = pow(c, d, n)

plaintext = plaintext_bytes = m.to_bytes(
    (m.bit_length() + 7) // 8, byteorder='big').decode('utf-8')

print(plaintext)

```
[🏠 Back to Main Page](https://github.com/Greenest-Guy/RSA-CTF-Writeups)
