# Web Stack

## TCP/IP

**TCP/IP is a network protocol, that is, a *set of established rules* two computers have to follow to get connected over a physical network to exchange messages.** TCP/IP is composed of two different protocols covering two different layers of the OSI stack, namely the Transport (TCP) and the Network (IP) ones. **TCP/IP can be implemented on top of any physical interface (Data Link and Physical OSI layers), such as Ethernet and Wireless. Actors in a TCP/IP network are identified by a *socket*, which is a tuple made of an IP address and a port number.**

As far as we are concerned when developing a Web service, however, we need to be aware that TCP/IP is a *reliable* protocol, which in telecommunications means that the protocol itself takes care or retransmissions when packets get lost. In other words, while the speed of the communication is not granted, we can be sure that once a message is sent it will reach its destination without errors.

## HTTP

TCP/IP can guarantee that the raw bytes one computer sends will reach their destination, but this leaves completely untouched the problem of how to send meaningful information. HTTP is the protocol that was devised to solve such a problem and has since greatly evolved.

At its core, **HTTP is a protocol that states the format of a text request and the possible text responses.** The most important feature of HTTP we need to keep in mind as developers is that it is a stateless protocol. This means that the protocol doesn't require the server to keep track of the state of the communication between requests, basically leaving session management to the developer of the service itself.

Session management is crucial nowadays because you usually want to have an authentication layer in front of a service, where a user provides credentials and accesses some private data. It is, however, useful in other contexts such as visual preferences or choices made by the user and re-used in later accesses to the same website. Typical solutions to the session management problem of HTTP involve the use of cookies or session tokens.

## HTTPS

HTTP is inherently insecure, being a plain text communication between two servers that usually happens on a completely untrustable network such as the Internet. While security wasn't an issue when the protocol was initially conceived, it is nowadays a problem of paramount importance, as we exchange private information, often vital for people's security or for businesses. We need to be sure we are sending information to the correct server and that the data we send cannot be intercepted.

HTTPS solves both the problem of tampering and eavesdropping, encrypting HTTP with the Transport Layer Security (TLS) protocol, that also enforces the usage of digital certificates, issued by a trusted authority.

## WebSocket

One great disadvantage of HTTP is that communication is always initiated by the client and that the server can send data only when this is explicitly requested. Polling can be implemented to provide an initial solution, but it cannot guarantee the performances of proper full-duplex communication, where a channel is kept open between server and client and both can send data without being requested. Such a channel is provided by the WebSocket protocol.

WebSocket is a killer technology for applications like online gaming, real-time feeds like financial tickers or sports news, or multimedia communication like conferencing or remote education.

It is important to understand that WebSocket is not HTTP, and can exist without it. It is also true that this new protocol was designed to be used on top of an existing HTTP connection, so a WebSocket communication is often found in parts of a Web page, which was originally retrieved using HTTP in the first place.

## Web Frameworks

The role of the Web framework is that of *converting HTTP requests into function calls*, and function return values into HTTP responses. The framework's true nature is that of a layer that connects a working business logic to the Web, through HTTP and related protocols. The framework takes care of session management for us and maps URLs to functions, allowing us to focus on the application logic.

In the grand scheme of an HTTP service, this is what the framework is supposed to do. Everything the framework provides out of this scope, like layers to access DBs, template engines, and interfaces to other systems, is an addition that you, as a programmer, may find useful, but is not in principle part of the reason why we added the framework to the system. We add the framework because it acts as a layer between our business logic and HTTP.

## Web Servers

The term “web server” can mean 1) a computer that stores the code for a website or 2) a program that runs on such a computer.

The physical computer device that we call a web server (much like the one shown below) is connected to the internet, and stores the code for different websites to be shared across the world at all times. When we load the code for our websites, or web apps, on a server like this, we would say that the server is “hosting” our website/app.

However, the server itself needs some code to tell *it* what to do, right? That software program is called… a web server! Its main purpose is to “serve” web pages it retrieves from your project code to users upon request.

## URLs

A URL is a formatted text string referring to the location of a resource on a computer network (most commonly the web). Typically, these resources are web pages, but they can also be text documents, graphics, programs, or pretty much anything that can be stored digitally.

![Web%20Stack%20a54ef298608c40a1b8b52e7c5add5c68/Untitled.png](Web%20Stack%20a54ef298608c40a1b8b52e7c5add5c68/Untitled.png)

## Static Vs. Static w/APIs Vs. Dynamic

Deploying a static web app is a little different than deploying a dynamic web app. Most of the apps that you have built up to this point are static apps. A basic definition of a static site is one that has hardcoded data that doesn’t change. A lot of our React apps use data from third-party libraries abut still deploy like a static app.

Deploying a site with an accompanying server and database is going to be more complicated than just an *almost* static site that consumes a third-party API.