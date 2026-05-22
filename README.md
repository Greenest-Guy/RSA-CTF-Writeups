<div align="center">

# 🔐 RSA CTF Writeups

*A collection of RSA cryptography write-ups from PicoCTF/CyLab Security Academy*

</div>

## 📖 About

This repository contains detailed write-ups for various RSA focused cryptography challenges from the CyLab Security Academy. Each write-up walks through the challenge set-up, steps to solve, and technical explanation on the mathematical vulnerability exploited.

## 🚩 Challenges

### Easy
- [```StegoRSA```](https://github.com/Greenest-Guy/RSA-CTF-Writeups/blob/main/challenges/StegoRSA.md)

### Medium
- [```Crack the Power```](https://github.com/Greenest-Guy/RSA-CTF-Writeups/blob/main/challenges/Crack_the_Power.md)
- [```ClusterRSA```](https://github.com/Greenest-Guy/RSA-CTF-Writeups/blob/main/challenges/ClusterRSA.md)

### Hard
- [```rsa-pop-quiz```](https://github.com/Greenest-Guy/RSA-CTF-Writeups/blob/main/challenges/rsa-pop-quiz.md)
- [```miniRSA```](https://github.com/Greenest-Guy/RSA-CTF-Writeups/blob/main/challenges/miniRSA.md)

## 🧮 RSA Mathematics Basics
The security of the RSA cryptosystem lies in the difficulty of factoring a large modulus into its original prime factors. RSA keys are generated using large prime factors, and utilizing modular arithmetic to encrypt and decrypt messages.

### Key Generation

First, two large distinct prime numbers must be chosen, $p$ and $q$

Then the modulus $n$ can be constructed:

$$n = p \cdot q$$

[Euler's Totient](https://en.wikipedia.org/wiki/Euler%27s_totient_function) $\phi (n)$ is then calculated using the shortcut:

$$\phi (n) = (p - 1)(q - 1)$$

Next, the public exponent $e$ must be chosen so that:

$$1 < e < \phi (n) \land \gcd(e, \phi (n)) = 1$$

Finally, the private exponent $d$, used for decryption, is calculated as follows:

$$d = e^{-1} \mod \phi (n) $$

### Encryption & Decryption

A plaintext message $m$ is encrypted into the ciphertext $c$ using the public exponent $e$, and the modulus $n$:

$$c = m^e \mod n$$

Conversely, the ciphertext is decrypted using the private exponent $d$ and the modulus $n$:

$$m = c^d \mod n$$

### Why It Works
 
Since the private exponent $d$ is the modular inverse of the public exponent $e \mod \phi(n)$, we have:

$$e \cdot d \equiv 1 \pmod{\phi(n)}$$

Following Euler's theorem:

$$m^{\phi(n)} \equiv 1 \pmod{n}$$

For any $m$ coprime to $n$, which gives us:
 
$$c^d = (m^e)^d = m^{e \cdot d} = m^{k \cdot \phi(n) + 1} \equiv m \pmod{n}$$
