# Satoshi's Have user-service

Authentication and user management functions for the Satoshi's Hive project.

## Prerequisites

Access to an LND service with a publically available gRPC address. This can be either on clearnet or tor (.onion). You'll also need the admin macaroon and tls cert for the node. These are normally found under `lnd/data/main/public/admin.macaroon` and `lnd/data/main/public/tls.cert`, depending on your node setup. If the location of these files is not accessible from the location where you run the server, you can either:

1. Copy the files to the server location, and give the local path in the `.env` file.
2. Convert the files to hex, and provide these in the `.env` file. To convert to hex, run

```
cat <filename> | xxd -ps -c 200 | tr -d '\n'
```

Mongodb docker image for storing extended user account information.

## Server setup

Once you have downloaded or cloned this repository, run `npm install`in the server directory.
Unless you are running this code on a machine with an external domain, you'll need to temporarily expose it to the outside world, if order for
the Lightning wallet to make the API request back to the service. I suggest using ngrok (ngrok.io). Create a free account, download the executable and run it in a command prompt before starting the server below.

```
cd server

npm install
```

Create a `.env` file updated with values in `.env.sample` file
Start `ngrok` with the port number used in the `.env` file, e.g.

```
ngrok http 5001
```

Take the https url from the `forwarding` line (`https://******.ngrok.io`) and paste it into the `SERVER_HOST_URL` field in `.env`. Note that this will change every time you run ngrok.

Run the web server with the following command

```
npm run start
```

You should see the following output on your command line

```
Server Running at https://localhost:5001
```

## Running via Docker

As an alternative to building and running the client and service individually, you can run these via Docker. In the root of the project, simply run `docker_compose up`, and wait for it to build and start the `sh-user-service` and `mongo` containers. Once ready, the UI needs to be run separately.

## Usage

Once the client has started, you should see a `user connected` message in the _server_ logs, as the client connects to it over websocket. Click the "log in with lighning button", at which point a QR code should be displayed. Scan this with your lighning wallet, at which point, it should ask you if you want to log in to the domain of your temporary ngrok address. Select "Yes", if authentication is successful, you should be instantly routed to the Dashbaord page.
