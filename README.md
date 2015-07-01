# gnatsd
[![License MIT](https://img.shields.io/npm/l/express.svg)](http://opensource.org/licenses/MIT)
[![Build Status](https://travis-ci.org/apcera/gnatsd.svg?branch=master)](http://travis-ci.org/apcera/gnatsd)
[![Current Release](http://img.shields.io/badge/release-v0.5.6-1eb0fc.svg)](https://github.com/apcera/gnatsd/releases/tag/v0.5.6)
[![Coverage Status](https://img.shields.io/coveralls/apcera/gnatsd.svg)](https://coveralls.io/r/apcera/gnatsd?branch=master)

A High Performance [NATS](https://nats.io) Server written in [Go.](http://golang.org)

## Usage

```

Server options:
    -a, --addr HOST                  Bind to HOST address (default: 0.0.0.0)
    -p, --port PORT                  Use PORT (default: 4222)
    -P, --pid FILE                   File to store PID
    -m, --http_port PORT             Use HTTP PORT
    -c, --config FILE                Configuration File
    -ws, --websocket_port PORT       Use PORT for WebSocket connections
    -cores CPUCORES                  Number of CPU Cores to Use

Logging options:
    -l, --log FILE                   File to redirect log output
    -T, --logtime                    Timestamp log entries (default: true)
    -s, --syslog                     Enable syslog as log method.
    -r, --remote_syslog              Syslog server addr (udp://localhost:514).
    -D, --debug                      Enable debugging output
    -V, --trace                      Trace the raw protocol

Authorization options:
        --user user                  User required for connections
        --pass password              Password required for connections

Common options:
    -h, --help                       Show this message
    -v, --version                    Show version

```

## Configuration

```
# Sample config file

port: 4242
net: apcera.me # net interface

http_port: 8222

authorization {
  user:     derek
  password: T0pS3cr3t
  timeout:  1
}

cluster {
  host: '127.0.0.1'
  port: 4244

  authorization {
    user: route_user
    password: T0pS3cr3tT00!
    timeout: 0.5
  }

  # Routes are actively solicited and connected to from this server.
  # Other servers can connect to us if they supply the correct credentials
  # in their routes definitions from above.

  routes = [
    nats-route://user1:pass1@127.0.0.1:4245
    nats-route://user2:pass2@127.0.0.1:4246
  ]
}

# logging options
debug:   false
trace:   true
logtime: false
log_file: "/tmp/gnatsd.log"

#pid file
pid_file: "/tmp/gnatsd.pid"
```


## Building

This code currently requires at _least_ version 1.1 of Go, but we encourage
the use of the latest stable release.  Go is still young and improving
rapidly, new releases provide performance improvements and fixes.  Information
on installation, including pre-built binaries, is available at
<http://golang.org/doc/install>.  Stable branches of operating system
packagers provided by your OS vendor may not be sufficient.

Run `go version` to see the version of Go which you have installed.

Run `go build` inside the directory to build.

Run `go test ./...` to run the unit regression tests.

A successful build run produces no messages and creates an executable called
`gnatsd` in this directory.  You can invoke that binary, with no options and
no configuration file, to start a server with acceptable standalone defaults
(no authentication, no clustering).

Run `go help` for more guidance, and visit <http://golang.org/> for tutorials,
presentations, references and more.


## Client libraries

There are several client language bindings for NATS.
- [Go](https://github.com/apcera/nats)
- [Java](https://github.com/tyagihas/java_nats)
- [Java - Spring](https://github.com/mheath/jnats)
- [Node.js](https://github.com/derekcollison/node_nats)
- [Ruby](https://github.com/derekcollison/nats)

## WebSocket Client
In order to use websocket, the client must implement a PING/PONG based on natsd implmentation.
Server will send PING message on a defined interval (DEFAULT is 2 minutes)
After 2 (configurable) timeouts, server will disconnect the connection.
Server PING message is of the form 'PING\r\n'
WebSocket client must respond with 'PONG\n'


## License

(The MIT License)

Copyright (c) 2012-2015 Apcera Inc.

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to
deal in the Software without restriction, including without limitation the
rights to use, copy, modify, merge, publish, distribute, sublicense, and/or
sell copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS
IN THE SOFTWARE.
