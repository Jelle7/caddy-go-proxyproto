# caddy-go-proxyproto
Caddy wrapper for go-proxyproto

This repository is forked from `rocketreferrals` and adds Caddyfile support.

caddy listener wrapper `go_proxyproto` for Caddy 2 adds support for
PROXY headers using the go proxy proto listener https://github.com/pires/go-proxyproto

This caddy plugin makes it possible to listen to AWS NLB Proxy Protocol V2 requests.
The official proxy protocol package referenced on the caddy documentation does not
handle AWS NLB requests well. This is the only way to preserve client IP addresses
when putting caddy behind a NLB on AWS.

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
