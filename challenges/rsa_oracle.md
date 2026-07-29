# 🔐 rsa_oracle
**PicoCTF Difficulty:** Medium

**Files/Links Provided:** ```password.enc```, ```secret.enc```

## **Description**  
```
Can you abuse the oracle?

An attacker was able to intercept communications between a bank and a fintech company.
They managed to get the message (ciphertext) and the password that was used to encrypt the message.
```

## **Explanation**
$$c \equiv m^e \pmod n$$
$$c_a \equiv 2^e \pmod n$$
$$c_b \equiv c \cdot c_a \equiv m ^ e \cdot 2 ^ e \pmod n$$
$$(c_b) ^ d \equiv (m ^ e \cdot 2 ^ e) ^ d \equiv (m ^ e) ^ d \cdot (2 ^ e) ^ d \pmod n$$
$$(m ^ e) ^ d \equiv m \pmod n$$
$$(2 ^ e) ^ d \equiv 2 \pmod n$$
$$(c_b) ^ d \equiv m \cdot 2 \pmod n$$
$$m \equiv 2 ^ {-1} \cdot (c_b) ^ d \pmod n$$
$$m = \frac{(c_b)^d \bmod n}{2} \because 0 < 2m < n$$

## **Code / Commands / Images**
```Python
from pwn import *

c    = [REDACTED]
host = [REDACTED]
port = [REDACTED]

connection = remote(host, port)

# E --> encrypt D --> decrypt.
connection.send(b'E\n')

# enter text to encrypt (encoded length must be less than keysize):
connection.send(b'\x02\n')

data = connection.recvuntil(b"(m ^ e mod n) ")
c_a = int(connection.recvline().strip().decode())

# E --> encrypt D --> decrypt. 
connection.send(b'D\n')

# Enter text to decrypt:
payload = c * c_a
connection.send(str(payload).encode() + b"\n")

connection.recvuntil(b"(c ^ d mod n): ")
m = connection.recvline().strip().decode()

m = int(m, 16)//2

byte_length = (m.bit_length() + 7) // 8

password = m.to_bytes(byte_length, byteorder='big').decode("utf-8")

print(password)
````
