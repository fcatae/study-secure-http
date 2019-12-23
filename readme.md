HTTPS TLS
==========

## Keytool: How to generate certificates

    keytool -genkeypair -keystore self.jks -alias self



## OpenSSL: How to generate self-signed key

Rsa command:

    openssl -req -x509 -newkey rsa:2048 -keyout site.key -out site.crt

Complete command:

    openssl -req -x509 -nodes -days 365 -newkey rsa:2048 -keyout site.key -out site.crt

Validate private key:

    openssl rsa -in site.key -check

Validate certificate:

    openssl x509 -in site.crt -text


Create 