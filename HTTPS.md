## What is HTTPS?

Hypertext transfer protocol secure (HTTPS) is the secure version of [[HTTP]], which is the primary [[Protocol]] used to send data between a web browser and a website. HTTPS is encrypted in order to increase security of data transfer. This is particularly important when users transmit sensitive data, such as by logging into a bank account, email service, or health insurance provider.

## How does HTTPS work?

Here’s how the entire process works:

1. **Browser contacts website**: The user's web browser attempts to connect to a website using HTTPS

2. **SSL certificate sends**: The website's server responds by sending its SSL/[[TLS certificate]] to the browser. This certificate contains the website’s public key (encryption key) and is used to establish a secure connection.

![A website's server sending SSL/TLS certificate to a web browser attempting to connect to the website using HTTPS](https://static.semrush.com/blog/uploads/media/5e/fa/5efa02276f59f4edfebcc8254e75f88d/71dc95b01075afce7fdd48a7c0edfe75/haBJQA2lEdk-HxLfcvGx3ugI8R8uYdSzTYd3piGGBntFLww9rGEoUMzWxfGJIiav0AjpI-zwtp4zW7wkIyiwzln_qhgXvFN-OyHUg9rjPUY-21yVnz9Sd4sh_H6PpMd-t1Nfka65JRvOHGUXjg6REGg.png)

3. **Browser verifies certificate**: The browser checks the certificate to ensure it’s valid and is issued by a trusted certificate authority (like GoDaddy, DigiCert, Comodo, etc.). This step is crucial for confirming a website’s authenticity.

![Browser verifies SSL/TLS certificate](https://static.semrush.com/blog/uploads/media/9d/4e/9d4e677d3dd2f4131f2c9349ac7d59bc/b1c476982e7bd5a3c3b1094dedc97e33/wzVoLU_IhmwN3SC6Ne3YH1LqAPDGOgcYbWcT08_U3yYjR2z2H-18xem2QZDkxMIoNx65fISLBamUxbOI2iEIh3U3tMSnqVuJknAWXVJYL5bcFWAu-EZo0dKUso5jQGVfMPTgtTcOfGGJPFMEkjgNXx0.png)

4. **Encryption key exchange**: The browser and the server establish an encrypted connection by exchanging keys once the certificate is verified. The browser uses the server's public key to encrypt information, which can only be decrypted by the private key (i.e., the decryption key) the server holds.

![The browser and website's server exchange keys once the SSL/TLS certificate is verified](https://static.semrush.com/blog/uploads/media/ae/27/ae271d1b7da5cdf4857581cd2b0c868d/075b627cd2e4b32534c336f22922f22d/6e4b0ckg374hLCOtNo_zxOKjcETok65G9jkkCBJLP78pNn72w_alY-dkAzKRIkhQRi85R-iZhkuAjoVwkHcnv0HipoXA3tlbG-7lryK7yUqdYbvUKeuMIUoNBZ9mF8GhUOxDdAjQIvs6FF-6VxVS0j8.png)

5. **Encrypted data transfer**: All data transferred between the browser and the server is encrypted after the secure connection is established. Which ensures it can’t be read by anyone intercepting the data.

![The data between the browser and the server is encrypted](https://static.semrush.com/blog/uploads/media/4b/30/4b30307e90a5f5c7f6a727f552aeac75/ace5a77049e26168f5d88577e5c4a00e/0OlW8GCW0NoxzPo8OtRjXp-77MoDQfS46fg022xyy-woLoWx8jVcQTadNev91gm2mZ5NNBAJiCgTLoxp7Ln20ew0JqJ0n7661VkI4XTO1v9IY8VeiqgyyrI352RuoTXUcRgLvX6nDOU0HtqxyQ9n3YU.png)

6. **Data decryption and display**: The server decrypts the received data using the private key, processes it, and sends back the requested information. This data is also encrypted. The browser then decrypts the incoming data and displays the website content to the user.

![A browser displaying the website content to the user once the process is finished](https://static.semrush.com/blog/uploads/media/31/17/3117e66bc0a3e9c7df806ef5cd13cad6/cfac907acf4dc4cb99bfceac2e41d819/4WO4ydADsntrAKzhcPz8qtEGsG3_bxqzej_6PbZUfMn7u4VSTL9OSeSEoe_uErwHlkFWOJZl-refVE-nXuXMownsbISMfxhEzh20zD7irKROAhd80hA14DkpEfFPUBFB6TjytsdvItonTbNFmrzAw6g.png)

#### Before encryption:

```
This is a string of text that is completely readable
```

#### After encryption:

```
ITM0IRyiEhVpa6VnKyExMiEgNveroyWBPlgGyfkflYjDaaFf/Kn3bo3OfghBPDWo6AfSHlNtL8N7ITEwIXc1gU5X73xMsJormzzXlwOyrCs+9XCPk63Y+z0=
```
## What port does HTTPS use?

HTTPS uses port 443. This differentiates HTTPS from HTTP, which uses port 80.

## URL Format

A uniform resource locator ([[URL]]) serves as the address for locating resources on the internet. And it’s formatted slightly differently for HTTP and HTTPS. 

HTTPS URLs begin with “https://.” Which indicates a secure connection. 

But HTTP URLs start with “http://.” And the missing “s” signifies the absence of security