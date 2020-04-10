---
layout: post
title:  "How to create a self-signed certificate"
date:   2020-04-05 10:10:58 +0530
categories: Certificate
---

# Introduction:

A self signed certificate is certificate, which is not provided by trusted CA authorities like Digicert. In some cases it make sense to use Self signed certificate like dev environment or for intra net websites. Self signed certificate is used for a web application like apache, nginx etc, to make it run on HTTPS Service. In this blog we will use `openssl` and `keytool` utilities to create Self Signed Certificate.

## Step 1: Create Root Certs:

### Create folder structure: 

```shell
 mkdir /root/ca
 cd /root/ca
 mkdir certs crl newcerts private
 chmod 700 private
 touch index.txt
 echo 1000 > serial
```
### Create Root CA configuration file:

Download or copy [Root CA configuration file]({{site.url}}/{{site.baseurl}}/files/root-ca-openssl.txt) content and save as `openssl.cnf` in `/root/ca` folder.

### Create Root Key: 

```shell
 cd /root/ca
 openssl genrsa -aes256 -out private/ca.key.pem 4096
```
 Output:
 Enter pass phrase for ca.key.pem: secretpassword
 Verifying - Enter pass phrase for ca.key.pem: secretpassword
```shell
 chmod 400 private/ca.key.pem
```
### Create Root Certificate: 

```shell
 cd /root/ca
 openssl req -config openssl.cnf \
      -key private/ca.key.pem \
      -new -x509 -days 7300 -sha256 -extensions v3_ca \
      -out certs/ca.cert.pem

 Enter pass phrase for ca.key.pem: secretpassword
 You are about to be asked to enter information that will be incorporated
 into your certificate request.
 -----
 Country Name (2 letter code) [XX]:GB
 State or Province Name []:England
 Locality Name []:
 Organization Name []:Alice Ltd
 Organizational Unit Name []:Alice Ltd Certificate Authority
 Common Name []:Alice Ltd Root CA
 Email Address []:

 chmod 444 certs/ca.cert.pem
```


### Verify the root certificate: 

```shell
 openssl x509 -noout -text -in certs/ca.cert.pem
```

## Step 2: Create Intermediate Certs:

### Create folder structure: 

```shell
 mkdir /root/ca/intermediate
 cd /root/ca/intermediate
 mkdir certs crl csr newcerts private
 chmod 700 private
 touch index.txt
 echo 1000 > serial
 echo 1000 > /root/ca/intermediate/crlnumber
```
### Create Intermediate CA configuration file:

Download or copy [Intermediate configuration file]({{site.url}}/{{site.baseurl}}/files/intermediate-ca-openssl.txt) content and save as `openssl.cnf` in `/root/ca/intermediate` folder.

### Create Intermediate Key: 

```shell
 cd /root/ca
 openssl genrsa -aes256 \
      -out intermediate/private/intermediate.key.pem 4096

 Enter pass phrase for intermediate.key.pem: secretpassword
 Verifying - Enter pass phrase for intermediate.key.pem: secretpassword

 chmod 400 intermediate/private/intermediate.key.pem
```

### Create Intermediate Certificate:

```shell
 cd /root/ca
 openssl req -config intermediate/openssl.cnf -new -sha256 \
      -key intermediate/private/intermediate.key.pem \
      -out intermediate/csr/intermediate.csr.pem

 Enter pass phrase for intermediate.key.pem: secretpassword
 You are about to be asked to enter information that will be incorporated
 into your certificate request.
 -----
 Country Name (2 letter code) [XX]:GB
 State or Province Name []:England
 Locality Name []:
 Organization Name []:Alice Ltd
 Organizational Unit Name []:Alice Ltd Certificate Authority
 Common Name []:Alice Ltd Intermediate CA
 Email Address []:


 openssl ca -config openssl.cnf -extensions v3_intermediate_ca \
      -days 3650 -notext -md sha256 \
      -in intermediate/csr/intermediate.csr.pem \
      -out intermediate/certs/intermediate.cert.pem

 Enter pass phrase for ca.key.pem: secretpassword
 Sign the certificate? [y/n]: y

 chmod 444 intermediate/certs/intermediate.cert.pem
```
### Verify the Intermediate certificate:

```shell
 openssl x509 -noout -text \
      -in intermediate/certs/intermediate.cert.pem

 openssl verify -CAfile certs/ca.cert.pem \
      intermediate/certs/intermediate.cert.pem

 intermediate.cert.pem: OK
```
### Create the certificate chain file

```shell
 cat intermediate/certs/intermediate.cert.pem \
      certs/ca.cert.pem > intermediate/certs/ca-chain.cert.pem
 chmod 444 intermediate/certs/ca-chain.cert.pem
```

## Step 3: Create Sign server and client certificate

### Create Key 

```shell
 cd /root/ca
 openssl genrsa -aes256 \
      -out intermediate/private/www.example.com.key.pem 2048
 chmod 400 intermediate/private/www.example.com.key.pem
```
### Create Certificate

```shell
 cd /root/ca
 openssl req -config intermediate/openssl.cnf \
      -key intermediate/private/www.example.com.key.pem \
      -new -sha256 -out intermediate/csr/www.example.com.csr.pem

 Enter pass phrase for www.example.com.key.pem: secretpassword
 You are about to be asked to enter information that will be incorporated
 into your certificate request.
 -----
 Country Name (2 letter code) [XX]:US
 State or Province Name []:California
 Locality Name []:Mountain View
 Organization Name []:Alice Ltd
 Organizational Unit Name []:Alice Ltd Web Services
 Common Name []:www.example.com
 Email Address []:

 cd /root/ca
 openssl ca -config intermediate/openssl.cnf \
      -extensions server_cert -days 375 -notext -md sha256 \
      -in intermediate/csr/www.example.com.csr.pem \
      -out intermediate/certs/www.example.com.cert.pem
 chmod 444 intermediate/certs/www.example.com.cert.pem
```

### Verify the certificate

```shell
 openssl x509 -noout -text \
      -in intermediate/certs/www.example.com.cert.pem

 openssl verify -CAfile intermediate/certs/ca-chain.cert.pem \
      intermediate/certs/www.example.com.cert.pem
```

### Deploy the certificate

- ca-chain.cert.pem
- www.example.com.key.pem
- www.example.com.cert.pem


### Step 4: Create JKS file

```shell
 cd intermediate
 openssl pkcs12 -export -in certs/www.example.com.cert.pem -inkey private/www.example.com.key.pem      -chain -CAfile certs/ca-chain.cert.pem -name "www.example.com" -out www.example.com.p12

 keytool -importkeystore -deststorepass <password> -destkeystore www.example.com.jks -srckeystore      www.example.com.p12 -srcstoretype PKCS12
```

Ref: [ssl-jks-creation][ssl-jks-creation]

### Step 5: To read or verify PKSC12 and JKS file 

```shell 
keytool -v -list -keystore www.example.com.jks
keytool -v -list -keystore www.example.com.p12
```


# FAQ`s

#### How to read CER file ?

```shell 
openssl req -noout -text -in www.example.csr.pem
```

#### Failed to updated DB TXT_DB error number 2
[Refer](https://stackoverflow.com/questions/9496698/how-to-revoke-an-openssl-certificate-when-you-dont-have-the-certificate)

#### How to include SAN part of CERT ?
[Refer](https://stackoverflow.com/questions/30977264/subject-alternative-name-not-present-in-certificate)

#### How to inclue SAN part of CSR ?
[Refer](https://geekflare.com/san-ssl-certificate/)

# Conclusion:

I hope this article, will help you create self signed certificate. Please comment if you face any issues. 


[ssl-jks-creation]: https://coderwall.com/p/3t4xka/import-private-key-and-certificate-into-java-keystore
[slef-signed-cert-creation]: https://jamielinux.com/docs/openssl-certificate-authority/introduction.html
