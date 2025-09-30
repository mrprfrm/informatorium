# xray Client setup on macOS

This guide shows how to install and run the **Xray client** on macOS, verify your connection, and test that traffic is going through your Xray network.

## 1. Install Xray

The simplest way is via [Homebrew](https://brew.sh):

```bash
brew install xray
```

Alternatively, you can download the latest release binary from [XTLS/Xray-core](https://github.com/XTLS/Xray-core/releases).

## 2. Validate Your Config

Before running, always test your configuration file:

```bash
xray run -test -c client.json
```

If the config is valid, Xray exits cleanly without errors.

## 3. Check Server Port Reachability

Make sure your server is reachable from macOS:

```bash
nc -vz 198.51.100.42 443
```

If the connection succeeds:

```
Connection to 198.51.100.42 port 443 [tcp/https] succeeded!
```

then the server is up and accepting connections.

## 4. Run the Client

Start the client with your config file:

```bash
xray run -c client.json
```

In the example config, the client exposes:

- **SOCKS5 proxy** on `127.0.0.1:10808`
- **HTTP proxy** on `127.0.0.1:10801`

## 5. Compare Your Direct IP vs Proxy IP

First check your real IP without proxy:

```bash
curl https://httpbin.org/ip
```

Example output:

```json
{
  "origin": "203.0.113.25"
}
```

Now check through Xray’s SOCKS5 proxy:

```bash
curl -x socks5h://127.0.0.1:10808 https://httpbin.org/ip
```

And via the HTTP proxy:

```bash
curl -x http://127.0.0.1:10801 https://httpbin.org/ip
```

Both should return the **VPS exit IP**, e.g.:

```json
{
  "origin": "198.51.100.42"
}
```

If the IP differs from your real one, you are successfully inside the Xray network.

## 6. Measure Latency Through the Tunnel

You can measure request latency (“ping-like” test) via SOCKS5:

```bash
curl -x socks5h://127.0.0.1:10808 -o /dev/null -s -w "%{time_total}\n" https://www.google.com
```

Example result:

```
0.447627
```

This is the time in seconds it took for the request to complete through the tunnel.
