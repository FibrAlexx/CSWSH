# CSWSH
This repository provides a basic explanation of the WebSocket origin vulnerability, its exploitation, and different ways to prevent it.

To understand this exploitation, it's important to first grasp what WebSocket is and how it works. As the name suggests, it refers to a socket for web communication.

WebSockets are a technology that enables bidirectional communication between a client and a server over a single socket. This differs from traditional HTTP requests but often works with similar headers.

Here is how WebSocket communication works:

    Client → Server
    
The client initiates a connection to the server by sending an HTTPS WebSocket handshake request.

    Server → Client
    
The server responds to the handshake with the status code 101 Switching Protocols.

    Server → Client
    
The server sends messages to the client.

    Client → Server
    
The client closes the WebSocket connection when the communication ends.

This exploitation mentions paying attention to the "origin", which suggests that the Origin header plays a key role here. This header can help validate requests and prevent unwanted interactions.

# Exploring the Origin Header
The exploitation hints at the importance of the Origin header. This header is sent during the WebSocket handshake and can help servers identify and validate the source of a request.

For instance, a basic handshake request might look something like this:

    GET /ws HTTP/1.1
    Host: example.com
    User-Agent: Mozilla/5.0
    Origin: http://example.com
    Sec-WebSocket-Version: 13
    Connection: Upgrade
    Upgrade: websocket

To test the server's handling of the Origin header, we can try sending modified requests with a variety of origins to observe the behavior.

# Testing Scenarios
Here are some example tests to explore the server's validation of the origin header:

Test 1: Valid Origin.
 We send the default request with the legitimate Origin header:

      Origin: http://example.com

 Expected result: The server should accept the request.

Test 2: Missing Origin.
 We attempt the request without an Origin header:

      Origin:

 Expected result: The server might reject the handshake, depending on its security policies.

Test 3: Malicious Origin.
 We intentionally modify the Origin header to an external or malicious domain:

      Origin: http://malicious-site.com

 Expected result: A secure server should reject the request. If the handshake still succeeds, this indicate a lack of      strict validation.

Test 4: Random or Invalid Origin.
 We send a completely invalid or malformed origin:

      Origin: invalid-origin

 Expected result: The server should reject the request

# Observations and Notes
From these tests, we can identify whether the server enforces strict validation of the Origin header during WebSocket handshakes. If the server accepts requests with inccorect or missing origins, it may be vulnerable to certain types of attacks, such as Cross-Site WebSocket Hijacking (CSWSH).

To mitigate such vulnerabilities:
  Always validate the Origin header and only allow trusted domains.
  Use CSRF tokens to ensure that actions are authorized.
  Favor the wss protocol (WebSocket Secure) for encrypted communication.

  
