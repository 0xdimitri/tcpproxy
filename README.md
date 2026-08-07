# TCP Proxy

A transparent TCP proxy for intercepting, logging, and modifying traffic between a client and a remote host. Useful for analyzing undocumented network protocols during security assessments.

## Features

- Bidirectional TCP forwarding with hex dump logging of every packet
- Pluggable `request_handler` / `response_handler` functions for traffic modification (fuzzing, injection, tampering)
- Threaded, supports multiple concurrent client connections
- `receive_first` mode for server-initiated protocols (e.g. FTP, SSH banners)

## Usage

```bash
./tcp-proxy.py [localhost] [localport] [remotehost] [remoteport] [receive_first]

# Example
./tcp-proxy.py 127.0.0.1 9000 10.12.132.1 9000 True
```

`receive_first` — `True` if the remote service sends data first, `False` if the client initiates.

## How It Works

Each accepted connection is handled in its own thread. `proxy_handler` opens a socket to the remote host and relays data both ways: reading, hex-dumping, and passing each buffer through the request/response handlers before forwarding. The session ends when either side stops sending data.

## Requirements

Python 3.x, standard library only.

## Disclaimer

Intended for authorized security testing on systems you own or have explicit permission to test.