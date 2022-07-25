# Create your own root CA

## 1. Setup root CA

**Prereqs:** Copy root-ca.conf and server.conf from this repo and amend both files to your needs.

### 1.1. Prepare directory:

    $ mkdir certs db private
    $ openssl rand -hex 16 > db/serial
    $ touch db/index

### 1.2. Create key and CSR
`openssl req -new -out root-ca.csr -keyout private/root-ca.key -config root-ca.conf`

### 1.3. Create Certificate
`openssl ca -selfsign -in root-ca.csr -out root-ca.crt -extensions ca_ext -config root-ca.conf`

## 2. Create certificate for webservice

### 2.1 Key
`openssl genrsa -out server.key 4096`

### 2.2. CSR
`openssl req -new -out server.csr -key server.key -config server.conf`

### 2.3. Zert
`openssl x509 -req -days 3650 -in server.csr -CA root-ca.crt -CAkey private/root-ca.key -CAcreateserial -out server.crt -extfile server.conf -extensions req_v3ext`

## 3. Debugging

### 3.1. Verify

**Verify Key:**
`openssl rsa -noout -text -in server.key`

**Verify Cert:**
`openssl x509 -noout -text -in cert.pem`

**verify CSR:**
`openssl req -noout -text -in server.csr`

**Verify Target:**
`openssl s_client -connect pve.fritz.box:443 -verify 3`

### 3.2. Webserver with openssl

`openssl s_server -key key.pem -cert cert.pem -accept 10443 -www`

`-HTTP/-WWW emulates Webserver`
`-http_server_binmode (with -WWW/-HTTP) serves binary files`

## 4. AOB

### 4.1. Remove Passphrase
`openssl rsa -in server.key -out server.key.insecure`

### 4.2. Fast selfsigned Certificate
`openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 5 -nodes`
