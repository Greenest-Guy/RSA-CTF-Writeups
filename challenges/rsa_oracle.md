# 🔐 rsa_oracle
**PicoCTF Difficulty:** Medium

**Files/Links Provided:** ```password.enc```, ```secret.enc```

## **Description**
```
Can you abuse the oracle?

An attacker was able to intercept communications between a bank and a fintech company.
They managed to get the message (ciphertext) and the password that was used to encrypt the message.

After some intensive reconassainance they found out that the bank has an oracle that was used to encrypt the password and can be found here [host + port]. Decrypt the password and use it to decrypt the message. The oracle can decrypt anything except the password.
```

## **Mathematical Explanation**
This challenge provides us with an encrypted file ```secret.enc``` containing the flag. ```password.enc``` contains the key for ```secret.enc```; however, the key in ```password.enc``` is encrypted using RSA, leaving us only with the ciphertext.

Luckily for us, we have access to an oracle that allows us to encrypt and decrypt messages freely using the same public and private keys, with the only stipulation being we cannot decrypt the ciphertext in ```password.enc``` itself. Therefore, we must find a way to leverage the oracle to compute the key in ```password.enc``` without decrypting the key itself, as the oracle prevents it.

The solution is to abuse the mathematically deterministic behavior of RSA to generate a ciphertext that is not equal to the ciphertext in ```password.enc```, while still maintaining the information stored in ```password.enc```.

We denote the encrypted key for ```secret.enc``` in ```password.enc``` as the ciphertext $c$, with the plaintext message being $m$, with our goal being to find the unencrypted key $m$.

The relationship between $c$ and $m$ in RSA is as follows:

$$c \equiv m^e \pmod n$$

However, because of the stipulation in this challenge, we cannot directly decrypt $c$. We must create a message $c_b$ that is not equal to $c$ but, after decryption, can be reconstructed into $m$.

We first encrypt our own message $c_a$ where $m = 2$ using the oracle:

$$c_a \equiv 2^e \pmod n$$

We can then create our chosen ciphertext $c_b$ by multiplying $c$ with $c_a$:

$$c_b \equiv c \cdot c_a \equiv m ^ e \cdot 2 ^ e \pmod n$$

Since $c_b$ does not equal $c$, the oracle allows us to decrypt $c_b$ so that:

$$(c_b) ^ d \equiv (m ^ e \cdot 2 ^ e) ^ d \equiv (m ^ e) ^ d \cdot (2 ^ e) ^ d \pmod n$$

Since $e$ and $d$ are modular multiplicative inverses of each other, we know that:

$$(m ^ e) ^ d \equiv m \pmod n$$
$$(2 ^ e) ^ d \equiv 2 \pmod n$$

Therefore:

$$(c_b) ^ d \equiv m \cdot 2 \pmod n$$

This allows us to recover $m$ by simply dividing the ciphertext by our chosen plaintext $2$:

$$m \equiv 2 ^ {-1} \cdot (c_b) ^ d \pmod n$$

Where in this case:

$$m = \frac{(c_b)^d \bmod n}{2} \because 0 < 2m < n$$

## **Steps to Solve**
1. Connect to the oracle using the provided host and port
2. Tell the oracle we would like to encrypt a message
3. Encrypt $m = 2$ or the hex $0x02$ using the oracle
4. Tell the oracle we would like to decrypt a message
5. Calculate $c_b$ by mulitplying $c$ provided in ```password.enc``` and $c_a$ returned by the oracle
6. Decrypt $c_b$
7. Convert the decrypted $c_b$ from hexadecimal to decimal and divide by 2 to get $m$
8. Convert $m$ into bytes and then into text
9. Using openssl and the password, decrypt ```secret.enc``` to obtain the flag

## **Code / Commands / Images**
```bash
openssl enc -aes-256-cbc -d -in secret.enc -k [password]
```
```Python
from pwn import *

c    = [REDACTED]
host = [REDACTED]
port = [REDACTED]

connection = remote(host, port)

# E --> encrypt D --> decrypt.
connection.send(b'E\n')

# enter text to encrypt (encoded length must be less than keysize):
connection.send(b'\x02\n') # IMPORTANT send the hex \x02 not the ascii character 2 which is hex 51

data = connection.recvuntil(b"(m ^ e mod n) ")
c_a = int(connection.recvline().strip().decode())

# E --> encrypt D --> decrypt. 
connection.send(b'D\n')

# Enter text to decrypt:
payload = c * c_a
connection.send(str(payload).encode() + b"\n")

# receive decrypted hex
connection.recvuntil(b"(c ^ d mod n): ")
m = connection.recvline().strip().decode()

m = int(m, 16)//2 # convert the hexadecimal output into decimal then divide by 2

byte_length = (m.bit_length() + 7) // 8 # convert integer to bytes

password = m.to_bytes(byte_length, byteorder='big').decode("utf-8") # convert bytes to text (utf-8)

print(password)
````
