```Python
from pwn import *

c    = [REDACTED]
host = [REDACTED]
port = [REDACTED]

connection = remote(host, port)

connection.send(b'E\n')

connection.send(b'\x02\n')

data = connection.recvuntil(b"(m ^ e mod n) ")
c_a = int(connection.recvline().strip().decode())

connection.send(b'D\n')

payload = c * c_a
connection.send(str(payload).encode() + b"\n")

connection.recvuntil(b"(c ^ d mod n): ")
m = connection.recvline().strip().decode()

m = int(m, 16)//2

byte_length = (m.bit_length() + 7) // 8

password = m.to_bytes(byte_length, byteorder='big').decode("utf-8")

print(password)
````
