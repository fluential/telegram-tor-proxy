# telegram-tor-proxy
Telegram over Tor Anonymizing Proxy

## This is simple docker MTProto Telegram over Tor Anonymizing Proxy.

This docker-compose setup will expose a Telegram over Tor Anonymizing Proxy via SOCK5 proxy chaining feature of a an excellent [mtg:2](https://github.com/9seconds/mtg/tree/v2) alongside with Faketls and so on. There is no adtag support.

## Features

- Enforce all traffic via tor
- DNS-over-HTTPS via [Cloudflare TOR hidden resolver](https://blog.cloudflare.com/welcome-hidden-resolver) instead of TOR's built-in DNS mechanism which can be insecure.
- Replay attack protection
- Domain fronting
- FakeTLS implementation - appears as "another" domain name for standard ssl clients
- Further proxy chaining

### Components

- Tor Socks Proxy
- [MTG Telegram MTProto Proxy](https://github.com/9seconds/mtg/tree/v2)
- Cloudflare dns-proxy for DNS-over-HTTPS
- Socat

To get access details:
```shell
# docker-compose exec mtg /mtg access -p 443 /config.toml
{
  "ipv4": {
    "ip": "x.y.z.f",
    "port": 443,
    "tg_url": "(...)",
    "tg_qrcode": "(...)",
    "tme_url": "(...)",
    "tme_qrcode": "(...)"
  },
  "secret": {
    "hex": "(...)",
    "base64": "(...)"
  }
}
```

## To run:
```shell
docker-compose up
```

## More
- [Telegram MTProto Proxy link generator](http://seriyps.ru/mtpgen.html)
