HTTPS TLS
==========

## Algorithms

Preferred:

ECDHE-ECDSA-AES256-GCM-SHA384: Elliptic Curve Diffie–Hellman (ECDH), Elliptic Curve Digital Signature Algorithm (ECDSA), AES 256 in Galois Counter Mode, (AES256-GCM), SHA384

https://www.acunetix.com/blog/articles/tls-ssl-cipher-hardening/

## Keytool: How to generate certificates

    keytool -genkeypair -keystore self.jks -keyalg rsa -alias self



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

