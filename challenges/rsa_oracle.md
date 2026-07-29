```Python
from pwn import *

c = [REDACTED]
ip = [REDACTED]
port = [REDACTED]

connection = remote(ip, port)

connection.send(b'E\n')

connection.send(b'\x02\n')

data = connection.recvlines(timeout=5)
for line in data:
    if "ciphertext" in str(line):
        c_a = int(str(line)[27:-1])

connection.send(b'D\n')

payload = c * c_a
connection.send(str(payload).encode() + b"\n")

connection.recvuntil(b"mod n): ")
m = connection.recvline().strip().decode()
m = int(m, 16)//2

byte_length = (m.bit_length() + 7) // 8

byte_data = m.to_bytes(byte_length, byteorder='big').decode("utf-8")

print(byte_data)

````
