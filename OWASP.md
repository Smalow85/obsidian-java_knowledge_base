### **The Top 10 OWASP vulnerabilities in 2021 are:**

- #### Injection
A code injection happens when an attacker sends invalid data to the web application with the intention to make it do something that the application was not designed/programmed to do.
Perhaps the most common example around this security vulnerability is the **SQL query consuming untrusted data**:

```sql
**String query = “SELECT * FROM accounts WHERE custID = ‘” + request.getParameter(“id”) + “‘”;**
```
Here are OWASP’s technical recommendations to prevent SQL injections:
- The preferred option is to use a safe API, which avoids the use of the interpreter entirely or provides a parameterized interface or migrate to use Object Relational Mapping Tools (ORMs). Note: Even when parameterized, stored procedures can still introduce SQL injection if PL/SQL or T-SQL concatenates queries and data, or executes hostile data with EXECUTE IMMEDIATE or exec().
- Use positive or “allowlist” server-side input validation. This is not a complete defense as many applications require special characters, such as text areas or APIs for mobile applications.
- For any residual dynamic queries, escape special characters using the specific escape syntax for that interpreter. Note: SQL structure such as table names, column names, and so on cannot be escaped, and thus user-supplied structure names are dangerous. This is a common issue in report-writing software.
- Use LIMIT and other SQL controls within queries to prevent mass disclosure of records in case of SQL injection.

- #### Broken authentication
A broken authentication vulnerability can allow an attacker to use manual and/or automatic methods to try to gain control over any account they want in a system – or even worse – to gain complete control over the system.
The second most common form of this flaw is allowing users to [brute force](https://sucuri.net/guides/what-is-brute-force-attack/) username/password combination against those pages.

**According to the OWASP web application contains broken authentication vulnerability if:**

- Permits automated attacks such as credential stuffing, where the attacker has a list of valid usernames and passwords.
- Permits brute force or other automated attacks.
- Permits default, weak, or well-known passwords, such as”Password1″ or “admin/admin.″
- Uses weak or ineffective credential recovery and forgot-password processes, such as “knowledge-based answers,” which cannot be made safe.
- Uses plain text, encrypted, or weakly hashed passwords.
- Has missing or ineffective multi-factor authentication.
- Exposes session IDs in the URL (e.g., URL rewriting).
- Does not rotate session IDs after successful login.
- Does not properly invalidate session IDs. User sessions or authentication tokens (particularly single sign-on (SSO) tokens) aren’t properly invalidated during logout or a period of inactivity.

**OWASP’s technical recommendations are the following:**

- Where possible, implement multi-factor authentication to prevent automated, credential stuffing, brute force, and stolen credential reuse attacks.
- Do not ship or deploy with any default credentials, particularly for admin users.
- Implement weak-password checks, such as testing new or changed passwords against a list of the top 10,000 worst passwords.
- Align password length, complexity and rotation policies with [NIST](https://pages.nist.gov/800-63-3/) 800-63 B’s guidelines in section 5.1.1 for Memorized Secrets or other modern, evidence-based password policies.
- Ensure registration, credential recovery, and API pathways are hardened against account enumeration attacks by using the same messages for all outcomes.
- Limit or increasingly delay failed login attempts. Log all failures and alert administrators when credential stuffing, brute force, or other attacks are detected.
- Use a server-side, secure, built-in session manager that generates a new random session ID with high entropy after login. Session IDs should not be in the URL. Ids should also be securely stored and invalidated after logout, idle, and absolute timeouts.

- #### Sensitive data exposure

**Examples of Sensitive Data:**
- Credentials
- Credit card numbers
- Social Security Numbers
- Medical information
- Personally identifiable information (PII)
- Other personal information

**How to Prevent Data Exposure:**
- Classify data processed, stored, or transmitted by an application.
- Identify which data is sensitive according to privacy laws, regulatory requirements, or business needs.
- Apply controls as per the classification.
- Don’t store sensitive data unnecessarily.
- Discard it as soon as possible or use PCI DSS compliant tokenization or even truncation. Data that is not retained cannot be stolen.
- Make sure to encrypt all sensitive data at rest.
- Ensure up-to-date and strong standard algorithms, protocols, and keys are in place; use proper key management.
- Encrypt all data in transit with secure protocols such as TLS with perfect forward secrecy (PFS) ciphers, cipher prioritization by the server, and secure parameters.
- Enforce encryption using directives like HTTP Strict Transport Security (HSTS).
- Disable caching for responses that contain sensitive data.  
    Store passwords using strong adaptive and salted hashing functions with a work factor (delay factor), such as Argon2, scrypt, bcrypt, or PBKDF2.
- Verify independently the effectiveness of configuration and settings.

- #### XML external entities (XXE)

XML External Entity attack is a type of attack against an application that parses XML input. This attack occurs when XML input containing a reference to an external entity is processed by a weakly configured XML parser.

Most XML parsers are vulnerable to XXE attacks by default. That is why the responsibility of ensuring the application does not have this vulnerability lays mainly on the developer.

**Some of the ways to prevent XML External Entity attacks, according to OWASP, are:**

- Whenever possible, use less complex data formats ,such as JSON, and avoid serialization of sensitive data.
- Patch or upgrade all XML processors and libraries in use by the application or on the underlying operating system.
- Use dependency checkers (update SOAP to SOAP 1.2 or higher).
- Disable XML external entity and DTD processing in all XML parsers in the application
- Implement positive (“allowlisting”) server-side input validation, filtering, or sanitization to prevent hostile data within XML documents, headers, or nodes.
- Verify that XML or XSL file upload functionality validates incoming XML using XSD validation or similar.
- SAST tools can help detect XXE in source code – although manual code review is the best alternative in large, complex applications with many integrations.

- #### Broken access control

**Examples of Broken Access Control:**
- Access to a hosting control / administrative panel
- Access to a server via FTP / SFTP / SSH
- Access to a website’s administrative panel
- Access to other applications on your server
- Access to a database

**There are things you can do to reduce the risks of broken access control:**
- With the exception of public resources, deny by default.
- Implement access control mechanisms once and reuse them throughout the application, including minimizing CORS usage.
- Model access controls should enforce record ownership, rather than accepting that the user can create, read, update, or delete any record. Note: For example, if a user logs in as “John,” he could only create, read, update or delete records associated with the ID of “John,” never the data from other users.
- Unique application business limit requirements should be enforced by domain models.
- Disable web server directory listing and ensure file metadata (e.g. .git) and backup files are not present within web roots.
- Log access control failures, alert admins when appropriate (e.g. repeated failures). Note: We recommend our [free plugin for WordPress websites](https://br.wordpress.org/plugins/sucuri-scanner/), that you can download directly from the official WordPress repository.
- Rate limit API and controller access to minimize the harm from automated attack tooling.
- JWT tokens should be invalidated on the server after logout.  
    Developers and QA staff should include functional access control units and integration tests.

- #### Security misconfigurations

**In order to prevent security misconfigurations:**

- A repeatable hardening process that makes it fast and easy to deploy another environment that is properly locked down. Development, QA, and production environments should all be configured identically, with different credentials used in each environment. Automate this process in order to minimize the effort required to set up a new secure environment.
- A minimal platform without any unnecessary features, components, documentation, and samples. Remove or do not install unused features and frameworks.
- A task to review and update the configurations appropriate to all security notes, updates, and patches as part of the patch management process. In particular, review cloud storage permissions.
- A segmented application architecture that provides effective and secure separation between components or tenants, with segmentation, containerization, or cloud security groups.
- Sending security directives to clients, e.g. Security Headers.
- An automated process to verify the effectiveness of the configurations and settings in all environments.

- #### Cross site scripting (XSS)
**Cross Site Scripting (XSS)** is a widespread vulnerability that affects many web applications. XSS attacks consist of injecting malicious client-side scripts into a website and using the website as a propagation method.
The risks behind XSS is that it allows an attacker to inject content into a website and modify how it is displayed, forcing a victim’s browser to execute the code provided by the attacker while loading the page.

*XSS is present in about two-thirds of all applications.*

**How to Prevent XSS Vulnerabilities**

*Preventive measures to reduce the chances of XSS attacks should take into account the separation of untrusted data from active browser content. OWASP guidelines gives some practical tips on how to achieve it:*

- Using frameworks that automatically escape XSS by design, such as the latest Ruby on Rails, React JS. Learn the limitations of each framework’s XSS protection and appropriately handle the use cases which are not covered.
- Escaping untrusted HTTP request data based on the context in the HTML output (body, attribute, JavaScript, CSS, or URL) will resolve Reflected and Stored XSS vulnerabilities. The [OWASP Cheat Sheet for XSS Prevention](https://owasp.org/www-project-cheat-sheets/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet) has details on the required data escaping techniques.
- Applying context-sensitive encoding when modifying the browser document on the client side acts against DOM XSS. When this cannot be avoided, similar context-sensitive escaping techniques can be applied to browser APIs as described in the [OWASP Cheat Sheet for DOM based XSS Prevention](https://owasp.org/www-project-cheat-sheets/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet).
- Enabling a [content security policy (CSP)](https://blog.sucuri.net/2023/04/how-to-set-up-a-content-security-policy-csp-in-3-steps.html) is a defense-in-depth mitigating control against XSS. It is effective if no other vulnerabilities exist that would allow placing malicious code via local file includes (e.g. path traversal overwrites or vulnerable libraries from permitted content delivery networks).

- #### Insecure deserialization

Every web developer needs to make peace with the fact that attackers/security researchers are going to try to play with everything that interacts with their application–from the URLs to serialized objects.

**How to Prevent Insecure Deserializations**

***The best way to protect your web application from this type of risk is not to accept serialized objects from untrusted sources.***
If you can’t do this, OWASP security provides more technical recommendations that you (or your developers) can try to implement:

- Implementing integrity checks such as digital signatures on any serialized objects to prevent hostile object creation or data tampering.
- Enforcing strict type constraints during deserialization before object creation as the code typically expects a definable set of classes. Bypasses to this technique have been demonstrated, so reliance solely on this is not advisable.
- Isolating and running code that deserializes in low privilege environments when possible.
- Logging deserialization exceptions and failures, such as where the incoming type is not the expected type, or the deserialization throws exceptions.
- Restricting or monitoring incoming and outgoing network connectivity from containers or servers that deserialize.
- Monitoring deserialization, alerting if a user deserializes constantly.

- #### Using components with known vulnerabilities

**How to Avoid Using Components with Known Vulnerabilities**

- Remove all unnecessary dependencies.
- Have an inventory of all your components on the client-side and server-side.
- Monitor sources like Common Vulnerabilities and Disclosures [(CVE)](https://cve.mitre.org/) and National Vulnerability Database [(NVD)](https://nvd.nist.gov/) for vulnerabilities in the components.
- Obtain components only from official sources.
- Get rid of components not actively maintained.
- Use virtual patching with the help of a [Website Application Firewall](https://sucuri.net/website-firewall/).