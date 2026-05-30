# 🔐 Sum-O-Primes
**PicoCTF Difficulty:** Hard

**Files/Links Provided:** ```gen.py```, ```output.txt```

## **Description**  

```
We have so much faith in RSA we give you not just the product of the primes, but their sum as well!
```

## **Steps to Solve**  
1. Download ```gen.py``` and ```output.txt```
2. Extract the hexadecimal values from ```output.txt``` for the sum of $p$ and $q$ represented by $x$, the modulus $n$, and the ciphertext $c$.
3. Retrieve the value for the public exponent $e$ from ```gen.py```.
4. Convert each respective hexadecimal value into their decimal (base 10) equivalent.
5. Solve for Euler's Totient $\phi (n)$ by algebraically rearranging the equation.
6. Calculate Euler's Totient $\phi (n)$ through substitution.
7. Determine the private exponent $d$ using Euler's Totient $\phi (n)$.
8. Decrypt the ciphertext $c$.

## **Explanation**
This challenge provides us with a standard RSA setup, with one additional piece of information. Along with the modulus $n = pq$, we are given the sum of the two prime numbers $x = p + q$.

To decrypt the ciphertext $c$ we need to know the private exponent $d$:

$$m = c^d \mod n$$

The private exponent $d$ can be calculated as follows:

$$d = e^{-1} \mod \phi (n) $$

Surprisingly, we have enough information to calculate the value for $d$, using only the modulus $n$, the public exponent $e$, and the sum of the primes $x$, which can be proven algebraically.

We have all the values necessary to calculate $d$, aside from Euler's Totient $\phi (n)$.

We know that Euler's Totient $\phi (n)$ can be calculated using the equation:

$$\phi (n) = (p - 1)(q - 1)$$

After expanding the equation for Euler's Totient $\phi (n)$, we are left with:

$$\phi (n) = pq - p - q + 1$$

Since we know the modulus $n$ can be represented by the equation $n = pq$, we can say that:

$$\phi(n) = pq - p - q + 1 = n - p - q + 1$$

Additionally, we can factor out $-1$ from $- p - q$ so that:

$$\phi(n) = n - p - q + 1 = n - (p + q) + 1$$

Since we are aware of the sum of the prime numbers $x = p + q$, we can substitute $x$ for $p+q$ leaving us with:

$$\phi(n) = n - x + 1$$

Using this final equation we can calculate Euler's Totient $\phi (n)$, allowing us to calculate the private exponent $d$:

$$d = e^{-1} \mod \phi (n) $$

And finally decrypt the ciphertext $c$:

$$m = c^d \mod n$$

Note, an alternative solution involves finding the roots of a quadratic equation $t^2-xt+n = 0$, though this is more computationally intensive, and not my original solution.

## **Code / Commands / Images**
```Python
def from_hex(h):
    return int(h, 16)


sum_of_primes = from_hex("REDACTED")
n = from_hex("REDACTED")
c = from_hex("REDACTED")

e = 65537

phi_n = n - sum_of_primes + 1

d = pow(e, -1, phi_n)

m = pow(c, d, n)

plaintext_bytes = m.to_bytes(
    (m.bit_length() + 7) // 8, byteorder='big')

plaintext = plaintext_bytes.decode('utf-8')

print(plaintext)

```
> 

[🏠 Back to Main Page](https://github.com/Greenest-Guy/RSA-CTF-Writeups)
