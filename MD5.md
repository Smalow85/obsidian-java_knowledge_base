MD5 (Message Digest Method 5) is a cryptographic hash algorithm used to generate a 128-bit digest from a string of any length. It represents the digests as 32 digit hexadecimal numbers.

![md5_1-MD5_Algorithm](https://www.simplilearn.com/ice9/free_resources_article_thumb/md5_1-MD5_Algorithm.PNG)
### Advantages of MD5

![md5adv.](https://www.simplilearn.com/ice9/free_resources_article_thumb/md5adv.PNG)

- Easy to Compare: Unlike the latest hash algorithm families, a 32 digit digest is relatively easier to compare when verifying the digests.
- Storing Passwords: Passwords need not be stored in plaintext format, making them accessible for hackers and malicious actors. When using digests, the database also gets a boost since the size of all hash values will be the same.

- Low Resource: A relatively low memory footprint is necessary to integrate multiple services into the same framework without a CPU overhead.
- Integrity Check: You can monitor file corruption by comparing hash values before and after transit. Once the hashes match, file integrity checks are valid, and it avoids data corruption.
## Why is MD5 no longer secure?

### 1 – Brute force attacks on MD5 hashes are fast

A brute force attack is a way to find a password by trying many possibilities.  
Either by guessing what the user could have used (birthdate, the child’s names, pet names, …), or by trying everything (from a,b,c to 10 characters passwords with special characters). 20 years ago, it could take years to find a password for the world’s most powerful computers  
Today, everyone has a super-computer at home, with improvements in the processor and graphics processor,  we can decrypt “secure” passwords in a few days maximum.
### 2 – MD5 dictionary tables are big

**On MD5Online we like the dictionary tables.  
By storing over 1,150 billion passwords in our database, [we can give you an answer in a few seconds](https://www.md5online.org/md5-decrypt.html) for any hash**.

That’s the second problem with the MD5 algorithm.  
It is so widely used that huge databases like this have been created over the years.  
If your password is inside (and there is a good chance if you have a “short” password), your accounts are not safe at all.

As for the brute force method, the only way to be safe is to use a long random password with special characters.  
There are too many possibilities to have it in this kind of database.  
Database like this are taking a lot of disk space. Even if it’s cheaper and cheaper over the years, it’s still an obstacle.
### 3 – MD5 has collisions

The MD5 algorithm has also proven issues within its cryptographic method.  
A **collision** is when two words have the same hash generated.  
Safe algorithms have a good collision resistance.

That’s to say that you have low chances to get the same hash for different words.  
But MD5 has a low collision resistance.