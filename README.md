# caddy-go-proxyproto
Caddy wrapper for go-proxyproto

This repository is forked from `rocketreferrals` and adds Caddyfile support.

caddy listener wrapper `go_proxyproto` for Caddy 2 adds support for
PROXY headers using the go proxy proto listener https://github.com/pires/go-proxyproto

This caddy plugin makes it possible to listen to AWS NLB Proxy Protocol V2 requests.
The official proxy protocol package referenced on the caddy documentation does not
handle AWS NLB requests well. This is the only way to preserve client IP addresses
when putting caddy behind a NLB on AWS.

### Caddyfile

Load the listener before the tls wrapper in the global config section of your Caddyfile

```
{
    servers {
      listener_wrappers {
        go_proxyproto {
          timeout 5s
        }
        tls
      }
    }
    ...
}
```

### JSON

Load the listener before the tls wrapper

```js
{
  "apps": {
    "http": {
      "servers": {
        "myserver": {
          // ...
          "listener_wrappers":[{"wrapper": "go_proxyproto", "timeout": "5s"}, {"wrapper":"tls"}]
          // ...
        }
      }
    }
  }
}
```

### Installation

This is an example Dockerfile to build a caddy 2.4.6 Docker image containing this plugin.

```
FROM caddy:2.4.6-builder AS builder

RUN xcaddy build \
    --with github.com/Jelle7/caddy-go-proxyproto

FROM caddy:2.4.6

COPY --from=builder /usr/bin/caddy /usr/bin/caddy
```

This image can now be ran on an EC2 instance/EKS cluster behind an NLB and pass
client IP addresses correctly in the `X-Forwarded-For` header field.
