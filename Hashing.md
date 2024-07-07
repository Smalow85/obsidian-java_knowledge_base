Hashing consists of converting a general string of information into an intricate piece of data. This is done to scramble the data so that it completely transforms the original value, making the hashed value utterly different from the original.

![md5hashing](https://www.simplilearn.com/ice9/free_resources_article_thumb/md5hashing.PNG)

Hashing uses a hash function to convert standard data into an unrecognizable format. These hash functions are a set of mathematical calculations that transform the original information into their hashed values, known as the hash digest or digest in general. The digest size is always the same for a particular hash function like MD5 or SHA1, irrespective of input size.

Hashing has two primary use cases:
### Password Verification:

It is common to store user credentials of websites in a hashed format to prevent third parties from reading the passwords. Since hash functions always provide the same output for the same input, comparing password hashes is much more private.

![md5hashing1](https://www.simplilearn.com/ice9/free_resources_article_thumb/md5hashing1.PNG)

The entire process is as follows:

1. User signs up to the website with a new password
2. It passes the password through a hash function and stores the digest on the server
3. When a user tries to log in, they enter the password again
4. It passes the entered password through the hash function again to generate a digest
5. If the newly developed digest matches the one on the server, the login is verified
### Integrity Verification:

Some files can be checked for data corruption using hash functions. Like the above scenario, hash functions will always give the same output for similar input, irrespective of iteration parameters.

![md5hashing2.](https://www.simplilearn.com/ice9/free_resources_article_thumb/md5hashing2.PNG)

The entire process follows this order:

1. A user uploads a file on the internet
2. It also uploads the hash digest along with the file
3. When a user downloads the file, they recalculate the hash digest
4. If the digest matches the original hash value, file integrity is maintained