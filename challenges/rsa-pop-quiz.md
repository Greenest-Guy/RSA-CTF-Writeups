# 🔐 rsa-pop-quiz
**Difficulty:** Hard

## **Description**  

```
Class, take your seats! It's PRIME-time for a quiz...
```

## Question 1
### Given
- p
- q

### Calculate
- n

### Answer
Given the prime numbers $p$ and $q$, the modulus $n$ can be calculated as follows:

$$p * q = n$$


## Question 2
### Given
- p
- n

### Calculate
- q

### Answer
Given the prime number $p$ and the modulus $n$, the second prime number used to create the modulus can be calculated by rearranging $p * q = n$, so that:

$$p = n / q$$

## Question 3
### Given
- e
- n

### Calculate
- p
- q

### Answer
Given the public exponent $e$ and sufficiently large modulus $n$, the prime numbers in which the modulus is comprised of cannot be reasonably calculated due to the [RSA Problem](https://en.wikipedia.org/wiki/RSA_problem).

## Question 4
### Given
- p
- q

### Calculate
- $\phi (n)$

### Answer
Given the prime numbers $p$ and $q$ we can calculate [Euler's Totient](https://en.wikipedia.org/wiki/Euler%27s_totient_function) $\phi (n)$ using the shortcut:

$$\phi (n) = (p - 1)(q - 1)$$

## Question 5
### Given
- m
- e
- n

### Calculate
- c

### Answer
Given the message $m$, public exponent $e$, and modulus $n$, we can encrypt $m$ by using the public key $(n, e)$:

$$c = m^e \mod n$$

## Question 6
### Given
- c
- e
- n

### Calculate
- m

### Answer
Given the ciphertext $c$, public exponent $e$, and modulus $n$, we have the public key $(n, e)$ used for encryption, but are missing the private exponent $d$, meaning the ciphertext $c$ can not be decrypted.

## Question 7
### Given
- p
- q
- e

### Calculate
- d

### Answer
Given the two prime numbers $p$ and $q$, and the public exponent $e$, the private exponent $d$ can be calculated by calculating Euler's Totient.

$$\phi (n) = (p - 1)(q - 1)$$

$$d = e^(-1) \mod \phi (n) $$

[🏠 Back to Main Page](https://github.com/Greenest-Guy/RSA-CTF-Writeups)
