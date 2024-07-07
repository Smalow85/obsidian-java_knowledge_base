SHA stands for secure hashing algorithm. SHA is a modified version of MD5 and used for hashing data and [certificates](https://www.encryptionconsulting.com/digital-certificates/).

![[Pasted image 20240428225303.png]]

A hashing algorithm shortens the input data into a smaller form that cannot be understood by using bitwise operations, modular additions, and compression functions. You may be wondering, can hashing be cracked or decrypted? Hashing is similar to [[encryption]], the only difference between hashing and encryption is that hashing is one-way, meaning once the data is hashed, the resulting hash digest cannot be cracked, unless a brute force attack is used.

### **Different SHA Forms**

When learning about SHA forms, several different types of SHA are referenced. Examples of SHA names used are SHA-1, SHA-2, SHA-256, SHA-512, SHA-224, and SHA-384, but in actuality there are only two types: SHA-1 and SHA-2.

**SHA-1** was the original secure hashing algorithm, returning a 160-bit hash digest after hashing. Someone may wonder, can **SHA-2** be cracked like SHA-1? The answer is yes. Due to the short length of the hash digest, SHA-1 is more easily brute forced than SHA-2, but SHA-2 can still be brute forced. Another issue of SHA-1 is that it can give the same hash digest to two different values, as the number of combinations that can be created with 160 bits is so small. SHA-2 on the other hand gives every digest a unique value, which is why all certificates are required to use SHA-2.
SHA-2 can produce a variety of bit-lengths, from 256 to 512 bit, allowing it to assign completely unique values to every hash digest created. Collisions occur when two values have the same hash digest.

### **What SHA is used for and Why**

As previously mentioned, Secure Hashing Algorithms are required in all digital signatures and certificates relating to [[SSL and TLS]] connections, but there are more uses to SHAs as well.
Applications such as [SSH](https://www.encryptionconsulting.com/education-center/what-is-ssh/), S-MIME (Secure / Multipurpose Internet Mail Extensions), and IPSec utilize SHAs as well. SHAs are also used to hash passwords so that the server only needs to remember hashes rather than passwords.