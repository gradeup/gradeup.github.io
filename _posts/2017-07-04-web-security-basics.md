---
layout: post
title: Web Security Basics
---

At Gradeup, we organize weekly engineering sessions, where in our whole engineering team shares their experiences, rants about some technology or having a knowledge session over some topic.

This week, the topic for our session was __Web Security Basics__ being organized by Prashant Chaudhary [@pc9](https://github.com/pc9).

A brief summary of the talk is as follows

## Security

> Security - the elephant in the room
>
>    --- Rising Stack Blog [Nodejs Security Checklist](https://blog.risingstack.com/node-js-security-checklist/)

### Security HTTP Headers

There are a few security related HTTP headers, that we should set on a web application.

##### Strict-Transport-Security

It is a response header, abbreviated as HSTS, which tells the browser to communicate over HTTPS connections with the web server instead of using insecure HTTP connections.

##### X-Frame-Options

It is a response header, set by the domain from which the resource is being requested, which indicates the browser whether or not to render a web page in a frame, iframe or object. This helps in avoiding clickjacking attacks.

##### X-XSS-Protection

It is a feature of browser that stops websites from loading if they detect some cross side scripting attacks(XSS).

##### X-Content-Type-Options

It is a response header which indicates to the browser to strictly adhere to the [MIME type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types) specified in the [Content-Type](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type) header. To achieve this behavior, it is set to `nosniff` like

```
X-Content-Type-Options: nosniff
```

It is used for reducing MIME type security risks and prevents the browser from doing MIME-type sniffing.

##### Content-Security-Policy

It is a response headers, which is used for defining policies from where resources like media content, images, etc. can be loaded


### Web Security Attacks

##### ClickJacking

ClickJacking is a malicious technique which hijacks the clicks of a user on a website. It translates a user to click on to something malicious thereby sharing confidential information details he is not even aware of. The malicious code is generally hidden beneath legitimate buttons or other click-able content on a website. It is also known as User Interface redress attack.

The most common way to prevent this attack is by setting `X-Frame-Options` or `Content-Security-Policy` headers

##### Cross Side Scripting (XSS)

This attack is meant to inject client side script code into web-pages, thus giving unauthorized access to attackers of user cookies, session tokens, or other sensitive information retained by the browser.

It can be prevented by following:

* Properly sanitizing and escaping data collected from user

* Proper escaping html characters before displaying any sensitive data to user

##### Cross Site Request Forgery (CSRF) Attack

CSRF or XSRF is an attack which tricks a browser to perform an unwanted action in an application in a valid user session. It generally targets state changing requests.

It can be prevented by using the following:

* CSRF tokens, which are web server generated tokens unique to every session and every request.

* Same site cookies, cookies which are secure, HTTP only and sent to the same site.

##### Distributed Denial of Service (DDOS) Attack

A DDOS attack is an attempt to make a web application unavailable by overloading it with traffic from multiple comprised sources. The incoming traffic flooding the victim originates from many different sources, thereby making it difficult to stop the attack simply by blocking the IP. Moreover it is difficult to distinguish between the genuine and attack traffic.

It can be prevented by using a Third Party Provider DNS or System Hardening.

##### References

[MDN](https://developer.mozilla.org/en-US/)

[OWASP](https://www.owasp.org/index.php/Main_Page)

[Stack Overflow](https://stackoverflow.com)

[Wikipedia](https://en.wikipedia.org)
