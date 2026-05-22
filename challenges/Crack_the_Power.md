# 🔐 Crack the Power
**Difficulty:** Medium

**Files/Links Provided:** ```message.txt```

## **Description:**  

```
We received an encrypted message. The modulus is built from
primes large enough that factoring them isn’t an option, at
least not today. See if you can make sense of the numbers
and reveal the flag. Download the message.
```

## **Steps to Solve:**  
1. Download ```message.txt```
2. Extract $n$, $c$, and $e$ for analysis.
3. Since there is a small public exponent $e = 20$, and the ciphertext $c$ is relatively small, check if the encryption satisfies $m^e < n$
4. Seeing that $m^e < n$ is satisfied, we can conclude that $c$ corresponds to an exact integer $e$-th power. Therefore, we can recover $m$ by taking the $e$-th root of the ciphertext:
   
$$
m = \sqrt[e]{c}
$$

4. Use a Python script to calculate $m = \sqrt[e]{c}$ and convert the bytes to plaintext.

## **Explanation:**

This challenge uses RSA encryption where the modulus $n$ is built from primes too large to factor directly. However, the public exponent $e = 20$ is unusually small for standard RSA, which allows for the possibility of a low public exponent attack when the message $m$ is small enough to satisfy $m^e < n$.

The ciphertext in RSA is calculated as:

$$c = m^e \bmod n$$

Because of this, when $m^e < n$, modular reduction does not occur, therefore:

$$m^e < n \implies m^e \bmod n = m^e$$

This attack makes it so that decryption becomes basic exponentiation, allow us to recover the plaintext by taking the $e$-th root of the ciphertext $c$:

$$m = \sqrt[e]{c}$$

## **Code / Commands / Images**
```python
import gmpy2

# modulus & ciphertext redacted
n = (modulus)
c = (ciphertext)
e = 20

m, exact = gmpy2.iroot(c, e)

if exact:
    plaintext_bytes = m.to_bytes((m.bit_length() + 7) // 8, byteorder='big')
    plaintext = plaintext_bytes.decode('utf-8')

    print(plaintext)
```
> Python code to calculate the e-th root and decode the plaintext bytes to utf-8.

[🏠 Back to Main Page](https://github.com/Greenest-Guy/RSA-CTF-Writeups)
