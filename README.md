# http-https-tutorial

I apologise in advance for stealing of image content... this was thrown together in a hurry.


## Introduction

While discussing some issues that we were having on a cluster, we noticed that some of the team were lacking in a deeper understanding of how stuff works under-the-hood.

Hopefully, this quick and dirty primer should assist in getting people up to speed, or filling in the blanks.

## The Web

I assume that you know what the web is. You might have gone to google multiple times today already, checked your webmail or even started an incognito session for [reasons].

What underlies the web is a set of protocols/specifications - HTML, HTTP, HTTPS, DNS, TCP/IP, TLS and many many more.

I will try to show you how these fit together in the following sections.


## HTML

Hypertext Markup Language, or HTML for short, is at its' core simply a text file with some conversions about how certain things are written.

For example, the following is a valid but simple html document.

```html
<html>
  <head>
    <title> Hello World </title>
  </head>
  <body>
    <h1> This is a first level heading</h1>
    <p> This is a paragraph </p>
  </body>
</html>
```


## HTTP

HTTP or HyperText Transfer Protocol is a text-based protocol; text based as in you can view it and actually understand it without needing to convert it.

```http
GET / HTTP/1.1
Host: thetimes.co.uk
User-Agent: curl/7.79.1
Accept: */*
```

The first line is the only mandatory one in HTTP 0.9 and 1.0. This specifies the method - GET
The url being requested - /
and the HTTP version - HTTP/1.1

In version 1.1 the Host header is required. This allows a single IP address/port combination to be used for more than one domain. ie. 127.1.2.3 answering requests for foo.com and bar.net.

This functionality is used extensively by Alsop and Lacuna and is required to be able to use an ALB in AWS.

## HTTPS

HTTPS is at the simplest just HTTP with a encrypted transport. You can still see the exact same headers being used with the same meaning, but this is done over a secure connection.

The first part of the below output is the TLS (SSL) negotiation and you then see similar HTTP request headers to the normal HTTP version above. 

```http
* Connected to thetimes.co.uk (52.208.17.106) port 443 (#0)
* ALPN, offering http/1.1
* successfully set certificate verify locations:
*  CAfile: /etc/ssl/cert.pem
*  CApath: none
* (304) (OUT), TLS handshake, Client hello (1):
} [316 bytes data]
* (304) (IN), TLS handshake, Server hello (2):
{ [104 bytes data]
* TLSv1.2 (IN), TLS handshake, Certificate (11):
{ [4021 bytes data]
* TLSv1.2 (IN), TLS handshake, Server key exchange (12):
{ [333 bytes data]
* TLSv1.2 (IN), TLS handshake, Server finished (14):
{ [4 bytes data]
* TLSv1.2 (OUT), TLS handshake, Client key exchange (16):
} [70 bytes data]
* TLSv1.2 (OUT), TLS change cipher, Change cipher spec (1):
} [1 bytes data]
* TLSv1.2 (OUT), TLS handshake, Finished (20):
} [16 bytes data]
* TLSv1.2 (IN), TLS change cipher, Change cipher spec (1):
{ [1 bytes data]
* TLSv1.2 (IN), TLS handshake, Finished (20):
{ [16 bytes data]
* SSL connection using TLSv1.2 / ECDHE-RSA-AES128-GCM-SHA256
* ALPN, server accepted to use http/1.1
* Server certificate:
*  subject: CN=thetimes.co.uk
*  start date: Apr  2 11:00:09 2022 GMT
*  expire date: Jul  1 11:00:08 2022 GMT
*  subjectAltName: host "thetimes.co.uk" matched cert's "thetimes.co.uk"
*  issuer: C=US; O=Let's Encrypt; CN=R3
*  SSL certificate verify ok.

GET / HTTP/1.1
Host: thetimes.co.uk
User-Agent: curl/7.79.1
Accept: */*
```

## HTTP version 2

HTTP version 2 is a slightly different animal. Instead of being text based, it uses binary encoded headers. This is to allow it to be more compact. I will not cover this in detail here as right now it isn't important for this tutorial.

# Diving Deeper

## The protocol stack

![alt text](/7095.epsi.gif)

## Internet Protocol (IP)

You will have seen IP addresses around a lot; addresses such as 127.0.0.1, 1.1.1.1 or 8.8.8.8. This are just (generally) unique numbers that represent machines on the internet.

The IP header contains the source (often seen as src) and destination (dst) addresses, but itself doesn't use the concept of ports - for these you need to go deeper into TCP and UDP.

IP packets themselves have no concept of connections. Each packet stands alone. It is only higher level protocols that add these features.

![alt text](/tcp-ip%20headers.png)
## TCP

Under the hood, HTTP, HTTPS and similar protocols all use the idea of connections. You make a connection to a server and that connection is then used to send and receive data.

The protocol used to do this is called TCP.

![alt text](/tcp-header.png)

## DNS


## TLS/SSL

![alt text](http://url/to/img.png)

### Versions

![alt text](/0lb1buik85z6dkrn274b.png)
### Ciphers

![alt text](/tls-cipher-suite.png)
### SNI

![alt text](/client_hello.png)

### ALPN

![alt text](http://url/to/img.png)
