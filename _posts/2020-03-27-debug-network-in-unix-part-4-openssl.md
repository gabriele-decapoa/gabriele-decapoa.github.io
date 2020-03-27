---
layout: post
title: How to debug network issues in Unix - "openssl"
categories:
  - bash
  - Unix
  - network
---

Restarting with my personal network debug toolkit, today I will describe `openssl`.

As described by its [web site](https://www.openssl.org/), OpenSSL is a robust, commercial-grade, and full-featured toolkit for the Transport Layer Security (TLS) and Secure Sockets Layer (SSL) protocols, but also a general-purpose cryptography library.

With `openssl` it is possible to verify if a TLS/SSL certificate is still valid or not for a specific website, or retrieve the certificates, or verify ciphers, or verify which TLS version is supported.

## Test SSL certificate of particular URL
`echo | openssl s_client -connect yoururl.com:443 –showcerts`

## Check Certificate Expiration Date of SSL URL
`echo | openssl s_client -connect secureurl.com:443 2>/dev/null | openssl x509 -noout -startdate –enddate`

## Check if SSL V2 or V3 is accepted on URL
Depending on `openssl` version you are using, you could test if various SSL or TLS versions are enable for an URL.   
To check SSL v2
```
echo | openssl s_client -connect secureurl.com:443 -ssl2
```

To Check SSL v3
```
echo | openssl s_client -connect secureurl.com:443 –ssl3
```

To Check TLS 1.0
```
echo | openssl s_client -connect secureurl.com:443 –tls1
```

To Check TLS 1.1
```
echo | openssl s_client -connect secureurl.com:443 –tls1_1
```

To Check TLS 1.2
```
echo | openssl s_client -connect secureurl.com:443 –tls1_2
```

<!--- https://geekflare.com/openssl-commands-certificates/ -->
<!--- https://gist.github.com/shpatserman/3cfcb59e40cf4381a5cec79b700ad98c -->
