# caddy-go-proxyproto
Caddy wrapper for go-proxyproto

This repository is forked from `rocketreferrals` and adds Caddyfile support.

caddy listener wrapper `go_proxyproto` for Caddy 2 adds support for
PROXY headers using the go proxy proto listener https://github.com/pires/go-proxyproto

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
