# 🔐 StegoRSA
**Difficulty:** Easy

**Files/Links Provided:** ```flag.enc```, ```image.jpg```

## **Description**  

```
A message has been encrypted using RSA. The public key is gone…
but someone might have been careless with the private key.
Can you recover it and decrypt the message? Download the flag and image.
```

## **Steps to Solve**  
1. Download ```flag.enc```, ```image.jpg```
2. Analyze ```image.jpg``` using ExifTool
3. Copy the comment string hidden within ```image.jpg```
4. Decode the string from hexadecimal to plain text
5. Use the private RSA key recovered to decrypt ```flag.enc```

## **Explanation**
  The private key for ```flag.enc``` was carelessly stored as a hexadecimal comment in ```image.jpg```, allowing for the use of steganography to retrieve the key and decrypt the file.

## **Code / Commands / Images**
```bash
exiftool image.jpg
```
> Read metadata of image

```bash
echo "HEX" | xxd -r -p > key.txt
```
> Decode hexadecimal into ascii, storing the output in key.txt

```bash
openssl pkeyutl -decrypt -inkey key.txt -in flag.enc
```
> Decrypts ```flag.enc``` using the private RSA key in ```key.txt```

[🏠 Back to Main Page](https://github.com/Greenest-Guy/RSA-CTF-Writeups)
