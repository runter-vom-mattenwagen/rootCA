# OpenSSL

## Schnelles selfsigned Zertifikat

`openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 5 -nodes`

## CA einrichten

**Verzeichnis vorbereiten:**

    `$ mkdir certs db private`
    `$ openssl rand -hex 16 > db/serial`
    `$ touch db/index`

**Key und CSR erzeugen:** `openssl req -new -out root-ca.csr -keyout private/root-ca.key -config root-ca.conf`
**Zertifikate erzeugen:** `openssl ca -selfsign -in root-ca.csr -out root-ca.crt -extensions ca_ext -config root-ca.conf`

## Serverzertifikat Webserver erzeugen

**Key:**  `openssl genrsa -out server.key 4096`
**CSR:**  `openssl req -new -out server.csr -key server.key -config server.conf`
**Zert:** `openssl x509 -req -days 3650 -in server.csr -CA root-ca.crt -CAkey private/root-ca.key -CAcreateserial -out server.crt -extfile server.conf -extensions req_v3ext`

## Checks

**Key verifizieren:**  `openssl rsa -noout -text -in server.key`
**Cert verifizieren:** `openssl x509 -noout -text -in cert.pem`
**CSR verifizieren:**  `openssl req -noout -text -in server.csr`
**Ziel verifizieren:** `openssl s_client -connect pve.fritz.box:443 -verify 3`

## Webserver mit openssl

`openssl s_server -key key.pem -cert cert.pem -accept 10443 -www`

`-HTTP/-WWW emulates Webserver`
`-http_server_binmode (with -WWW/-HTTP) serves binary files`

## Zeug

**Passphrase entfernen:**  `openssl rsa -in server.key -out server.key.insecure`
**in Ubuntu einfügen:**    `cp cacert.pem /usr/local/share/ca-certificates/cacert.crt && update-ca-certificates`

## Links

https://www.golinuxcloud.com/create-certificate-authority-root-ca-linux/
http://wiki.cacert.org/FAQ/subjectAltName
https://www.wikihow.com/Be-Your-Own-Certificate-Authority
https://www.digitalocean.com/community/tutorials/how-to-set-up-and-configure-a-certificate-authority-ca-on-ubuntu-20-04
https://roll.urown.net/ca/ca_root_setup.html
https://www.feistyduck.com/library/openssl-cookbook/online/
