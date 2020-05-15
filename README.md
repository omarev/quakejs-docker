# QuakeJS with Nginx content server

To run the server simply
```sh
$ docker-compose up -d
```

`NOTE: You need to create base & assets volumes in advance.`

### Running multiple servers
You can run multiple servers on single IP address by changing `net_port` parameter. Servers can share the same content-server (assets).
To do so just copy `quakejs` service block with another name, adjust the ports section and `net_port` parameter.
Finally don't forget to create another server.cfg and point to it in the service block.

```
services:
  quakejs_ctf_1:
    image: tentaculo/quakejs-server
    depends_on:
      - content-server
    volumes:
      - base:/quakejs/base
    ports:
      - 27950:27950
    network_mode: "host"
    entrypoint: node build/ioq3ded.js +set fs_game baseq3 +set dedicated 2 +set net_port 27950 +set fs_cdn <public_ip>:9000 +exec server.cfg
    restart: always
```
