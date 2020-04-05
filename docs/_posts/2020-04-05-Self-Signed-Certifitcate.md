---
layout: post
title:  "Steps to create self-signed certificate"
date:   2020-04-05 10:10:58 +0530
categories: Certificate
---

### Step 1: Create Certs

This document helps in creation self signed certificate [slef-signed-cert-creation][slef-signed-cert-creation]

### Step 2: Create JKS file

```bash
cd intermediate
openssl pkcs12 -export -in certs/www.example.com.cert.pem -inkey private/www.example.com.key.pem -chain -CAfile certs/ca-chain.cert.pem -name "www.example.com" -out www.example.com.p12

keytool -importkeystore -deststorepass <password> -destkeystore www.example.com.jks -srckeystore www.example.com.p12 -srcstoretype PKCS12
```

Ref: [ssl-jks-creation][ssl-jks-creation]

### Step 3: To read PKSC12 and JKS file 

```bash 
keytool -v -list -keystore www.example.com.jks
keytool -v -list -keystore www.example.com.p12
```


# FAQ`s

#### How to read CER file ?

```bash 
openssl req -noout -text -in www.example.csr.pem
```

#### Failed to updated DB TXT_DB error number 2
[Refer](https://stackoverflow.com/questions/9496698/how-to-revoke-an-openssl-certificate-when-you-dont-have-the-certificate)

#### How to include SAN part of CERT ?
[Refer](https://stackoverflow.com/questions/30977264/subject-alternative-name-not-present-in-certificate)

#### How to inclue SAN part of CSR ?
[Refer](https://geekflare.com/san-ssl-certificate/)

[ssl-jks-creation]: https://coderwall.com/p/3t4xka/import-private-key-and-certificate-into-java-keystore
[slef-signed-cert-creation]: https://jamielinux.com/docs/openssl-certificate-authority/introduction.html