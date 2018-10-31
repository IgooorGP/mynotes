# Django Channels

Project which extends Django's WSGI server to a ASGI specification -- Async Server Gateway Interface.

WSGI: Python standard specification for SYNC apps;

ASGI: Python standard specification for BOTH SYNC + ASYNC apps. (WSGI backwards-compatible).

## HTTP vs Socket Connections

HTTP protocol runs on a socket but it cannot transfer data. Instead, it needs an underlying protocol to transfer which is the TCP.

Sockets is an API that most OS provide to talk to the network. Hence, sockets can be used with any transport layer, not just the TCP or UDP.

HTTP is a way to send or receive a message using the HTTP protocol specification. 

Web sockets, on the other hand, allows us to create a persistent connection to transfer data based on any transport layer implementation:

(PC1) <---- persistent ----->  (PC2)

## Django channels

Extends django abilities beyond the HTTP protocol. It creates an async layer underneath Django's core which allows us to handle connections and sockets async.


### Consumers

* When Django receives an HTTP request --> consults URLconf to check for the appropriate view function to handle

* Similarly, when Channels receive a WebSocket connection --> consults URLconf --> find appropriate consumer --> calls the consumer's functions

Good practice to prefix paths with something such as ```/ws/something```  to distinguish between HTTP connections from Websocket connections for its easier to deploy on production:

Set the production web server such as nginx to route requests based on path to:

1. production WSGI server such as Gunicorn + Django;

2. production ASGI server such as Daphne + Channels.

There are two types of consumers, both of which echos received messages to some client:

1. Sync Consumers: lower performance;
2. Async Consumers: greater performance but must avoid BLOCKING operations such as accessing Django model.