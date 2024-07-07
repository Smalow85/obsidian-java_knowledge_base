SOAP (Simple Object Access Protocol) is a message protocol that enables the distributed elements of an application to communicate. SOAP is flexible and independent, which enables developers to write SOAP application programming interfaces ([[API]]) in different languages while also adding features and functionality.
## SOAP building blocks and message structure example

Simple Object Access Protocol, as a specification, defines SOAP messages that are sent to web services and client applications. SOAP messages are XML documents that are comprised of the following three basic building blocks:

1. The SOAP **Envelope** encapsulates all the data in a message and identifies the XML document as a SOAP message.
2. The **Header** element contains additional information about the SOAP message. This information could be authentication credentials, for example, which are used by the calling application.
3. The **Body** element includes the details of the actual message that need to be sent from the web service to the calling application. This data includes call and response information.

The fault message is an optional fourth building block. If a SOAP fault is generated, it is returned as an HTTP 500 error. Fault messages contain a fault code, string, actor and detailt.

==Advantages of SOAP include the following:==

- **Platform- and operating system-independent.** SOAP can be carried over a variety of protocols, enabling communication between applications with different programming languages on both Windows and Linux.
- **Works on the HTTP protocol.** Even though SOAP works with many different protocols, HTTP is the default protocol used by web applications.
- **Can be transmitted through different network and security devices.** SOAP can be easily passed through [firewalls](https://www.techtarget.com/searchsecurity/definition/firewall), where other protocols might require a special accommodation.

==Disadvantages, however, include the following:==

- **No provision for passing data by reference.** This can cause synchronization issues if multiple copies of the same object are passed simultaneously.
- **Speed.** The data structure of SOAP is based on XML. XML is largely human-readable, which makes it fairly easy to understand a SOAP message. However, that also makes the messages relatively large compared to the Common Object Request Broker Architecture (CORBA) and its remote procedure call ([RPC](https://www.techtarget.com/searchapparchitecture/definition/Remote-Procedure-Call-RPC)) protocol that will accommodate binary data. Because of this, CORBA and RPC are faster.
- **Not as flexible as other methods.** Although SOAP is flexible, newer methods, such as RESTful architecture, use XML, [JavaScript Object Notation](https://www.theserverside.com/definition/JSON-Javascript-Object-Notation), [YAML](https://www.techtarget.com/searchitoperations/definition/YAML-YAML-Aint-Markup-Language) or any parser needed, which makes them more flexible than SOAP.

### SOAP request:

```xml
POST /InStock HTTP/1.1  
Host: www.example.org  
Content-Type: application/soap+xml; charset=utf-8  
Content-Length: nnn  
  
<?xml version="1.0"?>  
  
<soap:Envelope  
xmlns:soap="http://www.w3.org/2003/05/soap-envelope/"  
soap:encodingStyle="http://www.w3.org/2003/05/soap-encoding">  
  
<soap:Body xmlns:m="http://www.example.org/stock">  
  <m:GetStockPrice>  
    <m:StockName>IBM</m:StockName>  
  </m:GetStockPrice>  
</soap:Body>  
  
</soap:Envelope>
```

### SOAP response:

```xml
HTTP/1.1 200 OK  
Content-Type: application/soap+xml; charset=utf-8  
Content-Length: nnn  
  
<?xml version="1.0"?>  
  
<soap:Envelope  
xmlns:soap="http://www.w3.org/2003/05/soap-envelope/"  
soap:encodingStyle="http://www.w3.org/2003/05/soap-encoding">  
  
<soap:Body xmlns:m="http://www.example.org/stock">  
  <m:GetStockPriceResponse>  
    <m:Price>34.5</m:Price>  
  </m:GetStockPriceResponse>  
</soap:Body>  
  
</soap:Envelope>
```

