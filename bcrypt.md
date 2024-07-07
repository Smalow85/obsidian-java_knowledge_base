The bcrypt hashing function allows us to build a password security platform that scales with computation power and always hashes every password with a salt.

There are plenty of cryptographic functions to choose from such as the [[SHA]] family. However, one design problem with the `SHA` families is that they were designed to be **computationally fast**. How fast a cryptographic function can calculate a hash has an immediate and significant bearing on how safe the password is. Faster calculations mean faster brute-force attacks, for example. Modern hardware in the form of CPUs and GPUs could compute millions, or even billions, of SHA-256 hashes per second against a stolen database.



