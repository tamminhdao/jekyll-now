---
layout: page
title: How do computers communicate?
---

> *Tam: "I just realize that I know nothing about the internet!"*
>
> *Eric: "Oh, me neither. I just take advantage of it."*


To be able to write an http server from scratch, I spent some time learning about the Internet's communication protocols. This is a monster of a topic. I started googling `http protocol`, which led me to `TCP/IP` which in turn led to the `internet protocol suite` and the `OSI model`. It is very easy to have a zillion tabs opened while being utterly confused.

This blog is my attempt of explaining (to myself) `http` and `TCP/IP` without getting too bogged down by the low level details. There will be analogies, since that's my preferred way to understand new concepts. Disclaimer: I did not come up with these analogies, only hand-picked them from a variety of articles I read.


### The Internet

The internet is a bunch of computers talking to each other. Some computers have information stored on them to share with others, we call them servers. Most computers, yours and mine included, just want to acquire information, we call them clients. The clients are the one initiating a conversation; they reach out to the servers requesting information. The servers keep their ears out for requests and response to them.

To be able to communicate with each other, computers have to follow the same protocols. A protocol suite is a collection of protocols that are designed to work together. Pretty much everything on the web runs on the TCP/IP suite, which is named after the Transmission Control Protocol (TCP) and the Internet Protocol (IP).

TCP is a connection oriented protocol and is used to provides a reliable end to end connection. It has built in error checking and will re-transmit missing packets. IP is the main networking protocol. These two are not the only two protocols in the suite, but they are the most common ones.

![](/images/TCPmodel.png){:class="img-responsive"}

### Here comes the socket

Computers on TCP/IP network communicate through sockets.
A socket is one endpoint of a two-way communication channel, just like a phone is one end of a phone call between two parties.
A socket consists of an IP address and a port number. The IP address identifies the device while the port number identifies the application running on that device. Continuing with the phone analogy, if IP address is like a company's phone number, the port number is like a department's extension.

An useful IP address to know is `127.0.0.1` or `::1`. That is your computer's localhost.
Port number ranges from 0 to 65535. However, ports 0 through 1023 are reserved for operating system's use.


### Hypertext Transfer Protocol (HTTP)

HTTP is one of many methods of communication on the web. Others include SMTP, FTP, etc... but HTTP is arguably the most popular.
Let's say we want to google something. The communication goes like this:

1. Client makes TCP connection to server at www.google.com:80
2. Server at www.google.com:80 accepts the connection
3. Client sends over the HTTP request over a socket connection specifying what it wants
4. Server receives request,
          parse that request,
          figure out what it's asking for,
          fetch that data (or generate it dynamically),
          send it back
5. Client receives and parses response
6. Server closes connection

HTTP is stateless. Once a request is fulfilled, the server doesn't keep track of anything.
In order for to be understood, HTTP requests and responses have to follow certain formats.

*Anatomy of an http request*
![](/images/httpRequest.png){:class="img-responsive"}

*Anatomy of an http response*
![](/images/httpResponse.png){:class="img-responsive"}

The data that are sent back and forth between the server and the client is always a stream of bytes representing text, image, audio, etc... To tell the receiver what to do with these bytes, the sender specifies the media type of the source in the <a href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type">content-type header</a>.


### Resources
1. <a href="http://www.steves-internet-guide.com/internet-protocol-suite-explained/">The TCP/IP Protocol Suite Explained for Beginners</a>
2. <a href="https://docs.oracle.com/javase/tutorial/networking/sockets/index.html">All About Sockets</a>
3. <a href="http://aosabook.org/en/500L/a-simple-web-server.html">A Simple Web Server</a>
