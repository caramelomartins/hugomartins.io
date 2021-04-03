---
title: "Understanding SSL Chaining"
date: 2018-03-27T00:00:00+01:00
draft: false
summary: SSL is a mess, sometimes. It is increasingly important, yet it is also hellishly difficult to debug and, if not perfectly understood, it seems that it can also be a pain to implement correctly. With the rise of automated tooling to help with implementation and management of SSL (certificate-wise and more), we have seen an increase of encryption usage in our HTTP connections ([1], [2]).
categories:
    - cryptography
---

SSL is a mess, sometimes. It is increasingly important, yet it is also hellishly difficult to debug and, if not perfectly understood, it seems that it can also be a pain to implement correctly. With the rise of automated tooling to help with implementation and management of SSL (certificate-wise and more), we have seen an increase of encryption usage in our HTTP connections ([1], [2]).

What SSL exactly? Putting it simply, it is a cryptographic protocol that standardizes the way an encrypted connection between two nodes is established. The acronym itself stands for _Secure Sockets Layer_. When we say we are connecting with HTTPS, we are essentially asserting that our connection uses the HTTP protocol to exchange information, inside a SSL-compliant communication channel. It is important to mention that SSL, although commonly used to refer to a secure communication channel, has been deprecated in favor of TLS (_Transport Layer Security_). They are essentially the same thing, at a higher level, although there are obvious differences between the two, at lower level, and TLS has replaced SSL.

Recently, I came across an SSL configuration issue that was getting hard to debug. After turning Apache's logs upside down I was able to verify that, somewhere along the way, Apache was logging the following error:

> alert unknown ca

As with everything surrounding SSL, the error message is fairly cryptic, short and concise. I had no idea what to do with that error message. I pasted it on Google and hoped for the best. Google brought me a StackOverflow question [3] that wasn't exactly the same. Nonetheless I read it and the answer, after explaining the most probable reason, for the issue at hand, offered the following notice:

> Another reason might be that you've used the correct certificate but failed to add the necessary chain certificates.

This looked promising. I looked up my configuration and, sure enough, my Apache configuration was pointing at wrong file for `SSLCertificateChainFile`. This left me with a question: what is exactly is this chaining sorcery?

DNSimple has a very thorough explanation on the concept [4]:

> There are two types of [certificate authorities (CAs)](https://support.dnsimple.com/articles/what-is-certificate-authority): **root CAs** and **intermediate CAs**. In order for an SSL certificate to be trusted, that certificate must have been **issued by a CA that is included in the trusted store of the device that is connecting**.
>
> If the certificate was not issued by a trusted CA, the connecting device (eg. a web browser) will then check to see if the certificate of the issuing CA was issued by a trusted CA, and so on until either a trusted CA is found (at which point a trusted, secure connection will be established) or no trusted CA can be found (at which point the device will usually display an error).

Not _all_ CAs (_Certificate Authorities_), entities that issue digital certificates, are trusted by _all_ devices. At the same time, CAs that are not trusted by a device can still sign digital certificates, which means that, for any number of devices, there are an awful lot of certificates that might not be directly trustable.  For this reason, people came up with a smart way of managing this situation. Each device has a central repository (_TrustSstore_) which defines which CAs it trusts. When a device, usually through a browser, looks up on a specific endpoint (www.google.com, for example), it fetches the digital certificate of that endpoint. The CA that signed that certificate might not be a trusted entity on that device. When that happens, the device initiates an attempt to establish a connection, between the CA that issued a certificate and any CA that the device actually trusts, based on the relationship between the CA that issued a certificate and the parent CAs of that CA. This trust relationship, again from DNSimple, is defined as a "chain of trust" [5].

For th trust relationships to work, each certificate that is signed has a series of _intermediate_ certificates, issued by intermediate CAs that are supported by the root CAs. This allows each of those intermediate CAs to sign several other certificates that will, in the end, be trusted, given the "chain of trust" that exists to the original trusted CA. This allows to keep the number of root CAs at a low level, making it easier to validate and secure, while, at the same time, delegating and decentralizing the work of signing certificates on a higher number of nodes.

In my previous example, the fact that I had misconfigured `SSLCertificateChainFile` meant that Apache, who was communicating with my clients, was telling them that the certificate they were receiving had been signed by an intermediate CA, that my clients didn't recognized as trustworthy. By configuring it properly, Apache started sending the intermediate certificate, essentially saying "here's the certificate of the CA that signed this certificate" thus my clients were able to establish the "chain of trust" necessary to validate the connection. Of course, all of the inner workings of this are complex but I think this paints an interesting and complete high-level picture.

In the end, SSL chaining is a concept that has been devised to allow a complex environment of CAs to be intertwined while, theoretically, maintaining an easily traceable cluster of trustworthy CAs and certificates. As we have seen recently ([5], [6] and [7]), this "chain of trust" can be easily manipulated and is not as thorough as we sometimes would wish for but, for now, its probably the best we have and we should keep working towards a fully encrypted web.

# References

[1] https://www.wired.com/2017/01/half-web-now-encrypted-makes-everyone-safer/

[2] https://transparencyreport.google.com/https/overview?hl=en

[3] https://serverfault.com/questions/793260/what-does-tlsv1-alert-unknown-ca-mean

[4] https://support.dnsimple.com/articles/what-is-ssl-certificate-chain/

[5] https://support.dnsimple.com/articles/what-is-ssl-root-certificate/

[6] https://www.scmagazine.com/google-proposes-revoking-symantec-certs/article/646293/

[7] https://www.securityweek.com/google-wants-symantec-certificates-replaced-until-chrome-70

[8] https://www.cyberscoop.com/trustico-digicert-ssl-certificates-revoked/
