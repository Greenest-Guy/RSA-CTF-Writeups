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


[🏠 Back to Main Page](https://github.com/Greenest-Guy/RSA-CTF-Writeups)
