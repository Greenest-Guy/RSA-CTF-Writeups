# 🔐 ClusterRSA
**Difficulty:** Medium

**Files/Links Provided:** ```message.txt```

## **Description**  
```
A message has been encrypted using RSA, but this time something feels... more crowded than usual. Can you decrypt it?
Download the message.
```

## **Steps to Solve**  
1. 

## **Explanation**
  
## **Code / Commands / Images**
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
