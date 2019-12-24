HTTPS TLS
==========

## Algorithms

Preferred:

ECDHE-ECDSA-AES256-GCM-SHA384: Elliptic Curve Diffie–Hellman (ECDH), Elliptic Curve Digital Signature Algorithm (ECDSA), AES 256 in Galois Counter Mode, (AES256-GCM), SHA384

https://www.acunetix.com/blog/articles/tls-ssl-cipher-hardening/

## Keytool: How to generate certificates

    keytool -genkeypair -keystore self.jks -keyalg rsa -alias self

## Use PKCS#12

Use PKCS#12 format (.pfx)

    keytool -genkeypair -keystore cert.pfx -keyalg rsa -alias self -storetype PKCS12

## Certificate chain

keytool -genkeypair -keystore root.jks -alias root -ext bc:c
keytool -genkeypair -keystore ca.jks -alias ca -ext bc:c
keytool -genkeypair -keystore server.jks -alias server
 
keytool -keystore root.jks -alias root -exportcert -rfc > root.pem

keytool -keystore ca.jks -certreq -alias ca > ca.csr

keytool -storepass <storepass> -keystore ca.jks -certreq -alias ca |
    keytool -storepass <storepass> -keystore root.jks
    -gencert -alias root -ext BC=0 -rfc > ca.pem
keytool -keystore ca.jks -importcert -alias ca -file ca.pem
 
keytool -storepass <storepass> -keystore server.jks -certreq -alias server |
    keytool -storepass <storepass> -keystore ca.jks -gencert -alias ca
    -ext ku:c=dig,kE -rfc > server.pem
cat root.pem ca.pem server.pem |
    keytool -keystore server.jks -importcert -alias server

## OpenSSL: How to generate self-signed key

Rsa command:

    openssl -req -x509 -newkey rsa:2048 -keyout site.key -out site.crt

Complete command:

    openssl -req -x509 -nodes -days 365 -newkey rsa:2048 -keyout site.key -out site.crt

Validate private key:

    openssl rsa -in site.key -check

Validate certificate:

    openssl x509 -in site.crt -text


Debug
=======

SSL communication broken:

    SSL handshake error: no cipher suites in common

Choose one option:

- javax.net.debug=ssl
- javax.net.debug=all

