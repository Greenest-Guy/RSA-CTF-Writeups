# 🔐 college-rowing-team
**PicoCTF Difficulty:** Hard

**Files/Links Provided:** ```encrypted-messages.txt```, ```encrypt.py```

## **Description**  

```
I just joined my college's rowing team! To make a good first impression, I started sending my teammates positive automated messages every day. I even send them flags from time to time!
```

## **Explanation**

This challenge uses RSA encryption where the modulus $n$ is built from primes too large to factor directly. However, the public exponent $e = 3$ is unusually small for standard RSA, which allows for the possibility of a low public exponent attack when the message $m$ is small enough to satisfy $m^e < n$.

The ciphertext in RSA is calculated as:

$$c = m^e \bmod n$$

Because of this, when $m^e < n$, modular reduction does not occur, therefore:

$$m^e < n \implies m^e \bmod n = m^e$$

This attack makes it so that decryption becomes basic exponentiation, allow us to recover the plaintext by taking the $e$-th root of the ciphertext $c$:

$$m = \sqrt[e]{c}$$

```encrypted-messages.txt``` contains 12 messages, all with their own modulus and ciphertext. Because of this, it is much more efficient to iterate through the txt file and use Python to automatically decode each individual message using the low public exponent attack.

## **Code / Commands / Images**
```Python
import gmpy2


file_path = r"encrypted-messages.txt"

n = None
c = None

with open(file_path, mode='r') as file:
    for line in file.readlines():
        if "n:" in line:
            n = int(line[2:])

        elif "c:" in line:
            c = int(line[2:])

        if n and c:
            m, exact = gmpy2.iroot(c, 3)

            plaintext_bytes = m.to_bytes(
                (m.bit_length() + 7) // 8, byteorder='big')

            plaintext = plaintext_bytes.decode('utf-8')

            print(plaintext)

            n = None
            c = None

```
> 

[🏠 Back to Main Page](https://github.com/Greenest-Guy/RSA-CTF-Writeups)
