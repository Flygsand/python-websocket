# python-websocket
python-websocket is a Python/asyncore WebSocket client library.

## Why would I want to use WebSocket over plain TCP for my desktop app?

While the primary intention of the WebSocket protocol is to enable
"TCP-like" full-duplex communication from within a web apps, it also
provides a number of benefits (over plain TCP) for desktop apps:

 * the WebSocket client library is responsible for message parsing,
   and exposes a simple, event-driven API. 
 * WebSocket connections are firewall- and proxy-friendly as they 
   are simply upgraded HTTP connections.
 * HTTP cookies can be attached to the WebSocket handshake, where 
   they can be used for e.g. authentication.

## Usage

    def my_msg_handler(msg):
      print 'Got "%s"!' % msg

    socket = WebSocket('ws://example.com/demo', onmessage=my_msg_handler)
    socket.onopen = lambda: socket.send('Hello world!')
    
    try:
      asyncore.loop()
    except KeyboardInterrupt:
      socket.close()

## API reference

### WebSocket(url, protocol=None, cookie_jar=None, onopen=None, onmessage=None, onerror=None, onclose=None)
Returns a WebSocket connected to a remote host at the given `url`. 
In order to allow communication over this socket, `asyncore.loop()`
must be called by the client.

The optional `protocol` parameter can be used to specify the
sub-protocol to be used. By providing a `cookie_jar` (cookielib), 
appropriate cookies will be sent to the server.

The remaining parameters are callback functions that will be 
invoked in the following manner:

    onopen:    invoked when a connection to the remote host has been
               successfully established

    onmessage: invoked when a message is received (passing the
               received data as an argument to the callback 
               function)

    onerror:   invoked when a communication error occured (passing 
               an Exception instance as an argument to the callback 
               function). If onerror is not provided, the default 
               asyncore behavior is used (raising the exception)

    onclose:   invoked when the connection has closed _normally_ (by
               request from either client or server)

### WebSocket.send(data)
Sends `data` through the socket.

### WebSocket.close()
Closes the socket.
