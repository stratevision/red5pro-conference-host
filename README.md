# Red5 Pro Simple Conference Host

> Simple WebSocket Server for hosting Testbed Conference Example

This NodeJS based server is an example of a simple in-memory WebSocket server that allows participants in a conference to register and unregister to a room. It can be used with the [Conference Examples](https://github.com/red5pro/streaming-html5/tree/master/src/page/test/conference) from the Red5 Pro Testbed.

Upon connection to the socket with the required parameters, connections are registered and all parties associated with the defined room are notified on previously existing connections. Likewise, when connections are closed, all parties associated with the defined room are notified.

Required connection parameters and notifications are detailed later in this document.

> You can specify the endpoint for this server when using the [Conference Examples](https://github.com/red5pro/streaming-html5/tree/master/src/page/test/conference) using the `socket` query parameter, e.g., [http://localhost:5080/webrtcexamples/test/conference/socket=localhost:8001](http://localhost:5080/webrtcexamples/test/conference/?socket=localhost:8001)

# Requirements

* NodeJS v10+
* NPM 6+

> This project was developed with the latest NodeJS & NPM as of the time of this writing (April 15th, 2021).

# Installation

```sh
npm install
```

# Usage

```sh
npm run start
```

> Running the above will start the server on the default port of `8001`

Alternatively:

```sh
PORT=3000 node index.js
```

> Running the above will start the server on the defined port `3000`

# Clients

## Parameters

Any connecting clients should provide the following query paramters in the URL when establishing a `WebSocket`:

* `room` : the name of the room to join.
* `streamName` : the name of the stream that the joining participant will be broadcasting with.

## Payloads

Upon any successfuly connection, each client associated with a `room` will be notified of an updated in-memory list of `streamName`s.

> It is important to note that this list will be reflective of all streams that are currently "active" in the room. It will grow as more client connections are made and shrink as client connections are closed.

Each client will be notified of the object with the associated `room` and a `streams` list of a `JSON Array` listing of each `streamName`. Each `streamName` is a `String`.

**Example**

```
{
  room: 'room1',
  streams: ['stream1', 'stream2', 'stream3']
}
```
