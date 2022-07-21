# Red5 Pro Simple Conference Host

> Simple WebSocket Server for hosting Testbed Conference Example

This NodeJS based server is an example of a simple in-memory WebSocket server that allows participants in a conference to register and unregister to a room. It can be used with the [Conference Examples](https://github.com/red5pro/streaming-html5/tree/master/src/page/test/conference) from the Red5 Pro Testbed.

Upon connection to the socket with the required parameters, connections are registered and all parties associated with the defined room are notified on previously existing connections. Likewise, when connections are closed, all parties associated with the defined room are notified.

Required connection parameters and notifications are detailed later in this document.

> You can specify the endpoint for this server when using the [Conference Examples](https://github.com/red5pro/streaming-html5/tree/master/src/page/test/conference) using the `socket` query parameter, e.g., [http://localhost:5080/webrtcexamples/test/conference/socket=localhost:8001](http://localhost:5080/webrtcexamples/test/conference/?socket=localhost:8001)

# Requirements

* NodeJS v10+
* NPM 6+

```sh
curl -sL https://deb.nodesource.com/setup_14.x -o nodesource_setup.sh
sudo bash nodesource_setup.sh
sudo apt-get install -y nodejs
sudo apt-get install build-essential
sudo npm install forever -g
```

> This project was developed with the latest NodeJS & NPM as of the time of this writing (April 15th, 2021).

# Installation

```sh
npm install
```
# SSL

For most NodeJS implementations, it is necessary to generate SSL certificate files, which are converted into .crt and .key files to be stored in the `<service>/cert` folder.

## Using Let's Encrypt

The following can be run to install Let's Encrypt Certbot on Ubuntu (`snap` is included with most Ubuntu distributions by default)

1.	sudo snap install core; sudo snap refresh core
2.	sudo snap install --classic certbot
3.	sudo ln -s /snap/bin/certbot /usr/bin/certbot

To generate the cert, run `sudo certbot certonly --standalone --email <your-email> --agree-tos -d <server-fqdn>`  (for example: `sudo certbot certonly --standalone --email jessica@infrared5.com --agree-tos -d test01.red5.net`)

You will then need to copy the fullchain and privatekey to the cert directory of your application

```sh
sudo cp /etc/letsencrypt/archive/<server-fqdn>/fullchain1.pem ~/<nodejs-server>/cert/certificate.crt
sudo cp /etc/letsencrypt/archive/<server-fqdn>/privkey1.pem ~/<nodejs-server>/cert/privateKey.key
sudo chmod +r ~/<nodejs-server>/cert/*
```

>  (note: the number will increment as you renew, i.e., fullchain1.pem --> fullchain2.pem, etc.

Your index.js file then needs to be modified with the full path to the certificate and privateKey files (replace with the appropriate paths):

```js
if (useSSL) {
  cert = fs.readFileSync('/home/ubuntu/serverapp/cert/certificate.crt')
  key = fs.readFileSync('/home/ubuntu/serverapp/cert/privateKey.key')
  port = 443
```

# Usage

```sh
npm run start
```

or better yet,

```sh
forever start index.js
```

to view the log location and status of the running process, run `forever list`

> Running the above will start the server on the default port which is defined in index.js

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

Upon any successful connection, each client associated with a `room` will be notified of an updated in-memory list of every `streamName`.

> It is important to note that this list will be reflective of all streams that are currently "active" in the room. It will grow as more client connections are made and shrink as client connections are closed.

Each client will be notified of the object with the associated `room` and a `streams` list of a `JSON Array` listing of each `streamName`. Each `streamName` is a `String`.

**Example**

```
{
  room: 'room1',
  streams: ['stream1', 'stream2', 'stream3']
}
```

## Forever Commands

	•	forever start: starts a script as a daemon.
	•	forever stop: stops the daemon script by Id|Uid|Pid|Index|Script.
	•	forever stopall: stops all running scripts.
	•	forever restart: restarts the daemon script.
	•	forever restartall: restarts all running forever scripts.
