Scrypt is a slow-by-design [key derivation function](https://blog.boot.dev/cryptography/key-derivation-functions/ "key derivation function") designed to create strong cryptographic keys. Simply put, the purpose of the Scrypt hash is to create a fingerprint of its input data but to do it _very slowly_. A common use-case is to create a strong private key from a password, where the new private key is longer and more secure.

Let’s pretend your password is `password1234`. By using Scrypt, we can extend that deterministically into a 256-bit key:

```
password1234 -> 
AwEEDA4HCwQFAA8DAwwHDQwPDwUOBwoOCQACAgUJBQ0JAAYNBAMCDQ4JCQgLDwcGDQMDDgMKAQsNBAkLAwsACA==
```
