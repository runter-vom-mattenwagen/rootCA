# OpenSSL

## Fast selfsigned Certificate

`openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 5 -nodes`

## Setup root CA

## Prepare directory:

    $ mkdir certs db private
    $ openssl rand -hex 16 > db/serial
    $ touch db/index

## Create key and CSR
`openssl req -new -out root-ca.csr -keyout private/root-ca.key -config root-ca.conf`

## Create Certificate
`openssl ca -selfsign -in root-ca.csr -out root-ca.crt -extensions ca_ext -config root-ca.conf`

## Serverzertifikat Webserver erzeugen

**Key:**
`openssl genrsa -out server.key 4096`

**CSR:**
`openssl req -new -out server.csr -key server.key -config server.conf`

**Zert:**
`openssl x509 -req -days 3650 -in server.csr -CA root-ca.crt -CAkey private/root-ca.key -CAcreateserial -out server.crt -extfile server.conf -extensions req_v3ext`

## Debugging

**Verify Key:**
`openssl rsa -noout -text -in server.key`

**Verify Cert:**
`openssl x509 -noout -text -in cert.pem`

**verify CSR:**
`openssl req -noout -text -in server.csr`

**Verify Target:**
`openssl s_client -connect pve.fritz.box:443 -verify 3`

### Webserver with openssl

`openssl s_server -key key.pem -cert cert.pem -accept 10443 -www`

`-HTTP/-WWW emulates Webserver`
`-http_server_binmode (with -WWW/-HTTP) serves binary files`

## AOB

**Remove Passphrase:**
`openssl rsa -in server.key -out server.key.insecure`
