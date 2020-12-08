---
layout: post
title:  "How to set up SSL for a MongoDB Replica Set"
date:   2020-07-05 06:39:32 -0400

categories: DevOps MongoDB Databases
---

```
openssl genrsa -out server.key 2048
openssl req -new -key server.key -out server.csr
openssl req -nodes -x509 -days 365 -newkey rsa:2048 -keyout ca.key -out ca.crt
cat ca.key ca.crt > ca.pem
openssl x509 -req -in server.csr -CA ca.pem -CAkey ca.key -CAcreateserial -out server.crt -days 799 -sha256 -extfile extensions.ext
openssl x509 -req -in server.csr -CA ca.pem -CAkey ca.key -CAcreateserial -out server.crt -days 799 -sha256 -extfile extensions.ext
cat server.key server.crt > server.pem
openssl verify -verbose -CAfile ca.pem server.pem
```

```
$ cat extensions.ext
extendedKeyUsage = serverAuth,clientAuth,TLS Web Server Authentication,TLS Web Client Authentication
basicConstraints = CA:FALSE
keyUsage = nonRepudiation, digitalSignature, keyEncipherment
subjectAltName = @alt_names
[alt_names]
DNS.1=devdbunified.aws.businessinsider.com
```
