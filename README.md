# socket-tunnel

Tunnel HTTP connections via socket.io streams. Inspired by [localtunnel](https://github.com/localtunnel/localtunnel).

## Blog Post

[Read all about it](https://ericbarch.com/post/sockettunnel/)

## Server Usage

1. Clone this repo and cd into it
2. Update the Docker file to add a password to the command line (see notes below)
3. docker build -t socket-tunnel .
4. docker run -d -p 80:3000 --restart=always --name st-server socket-tunnel
5. Get a domain name (i.e. YOURDOMAIN.com)
6. Point your domain name's root A record at your server's IP
7. Point a wildcard (*) A record at your server's IP

Note : if you are hosting on a subdomain, you will need to start the server with the `--subdomain` option.
For instance if you are using `tunnel.example.com` the option will be `--subdomain tunnel` and the tunnels created will be sudomains of `tunnel.example.com`.

To do so, replace the following line in the `Dockerfile`:
~~~Dockerfile
CMD [ "node", "bin/server" ]
~~~

by
~~~Dockerfile
CMD [ "node", "bin/server", "--subdomain", "tunnel" ]
~~~

Even if you are not using a subdomain, you will need to modify the Dockerfile to add the option `--password` to protect your server instance :
~~~Dockerfile
CMD [ "node", "bin/server", "--password", "pony-excelsior-virtuoso"]
~~~

## Client CLI Usage

1. Start your http server that you'd like to expose to the public web (in this example we'll assume it's listening on 127.0.0.1:8000)
2. Clone this repo and cd into it
3. `npm i`
4. `node bin/client --server http://YOURDOMAIN.com --subdomain YOURSUBDOMAIN --hostname 127.0.0.1 --port 8000 --pwd pony-excelsior-virtuoso`
5. Browse to http://YOURSUBDOMAIN.YOURDOMAIN.com to see your local service available on the public internet

## Client API Usage

Assuming a web server running on 127.0.0.1:8000

1. Clone this repo into your project
2. `npm i`
3. In your project file, require the socket-tunnel api and call connect():

```JavaScript
const socketTunnel = require('./socket-tunnel/lib/api');

socketTunnel.connect('http://YOURDOMAIN.com', 'YOURSUBDOMAIN', '8000')
  .then(console.log)
  .catch(console.log);
```
4. Browse to http://YOURSUBDOMAIN.YOURDOMAIN.com to see your local service available on the public internet

## Client API Parameters

`socketTunnel.connect(remoteServer, desiredSubdomain, localPort, localHostname)` returns a promise which resolves to the requested URL/subdomain.

| Property         | Default     | Description                                        |
|------------------|-------------|----------------------------------------------------|
| remoteServer     | n/a         | IP address or hostname of the socket-tunnel server |
| desiredSubdomain | n/a         | Subdomain to request for this client               |
| localPort        | n/a         | Local port to tunnel to                            |
| localHostname    | '127.0.0.1' | Local host to tunnel to                            |

## Credits

Created by Eric Barch. Additional code provided by [these generous contributors](https://github.com/ericbarch/socket-tunnel/graphs/contributors).

## License

This project is licensed under the MIT License - see the LICENSE file for details
