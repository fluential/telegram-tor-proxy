[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2Ffluential%2Ftelegram-tor-proxy%2F&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)

# Telegram Tor Stealth Proxy
<img width="514" alt="mtprotor-logo-linetop-color" src="https://github.com/fluential/telegram-tor-proxy/assets/1957220/1bf3085a-9d5f-4b42-951f-12cfe2233d2a">

Telegram over Tor Anonymizing Proxy


## This is MTProto Telegram over Tor Anonymizing Stealth Proxy

This docker-compose setup will allow to connect to a Telegram over Tor Anonymizing Proxy using excellent [mtg:2](https://github.com/9seconds/mtg/tree/v2). It uses iptables to block and redirect all traffic accordingly via TOR.

It will allow you to:
  - expose a stealth and secure telegram proxy for a large group of people
  - originate on telegram servers from one of the tor exit nodes. Be nice because telegram does not run tor hidden services so you are at mercy of tor exit node operators.
  - hide the fact of using telegram
  - hide the fact of using tor, usually you would have to configure telegram client to connect directly to TOR socks port exposing tor usage to your local ISP
  - chose your desired exit geo location (adjust torrc config to set exit node `ExitNodes {GB}`)

## Features
- Enforce all traffic via tor
- DNS-over-HTTPS via [Cloudflare TOR hidden resolver](https://blog.cloudflare.com/welcome-hidden-resolver) instead of TOR's built-in DNS mechanism which can be insecure.
- Replay attack protection
- Domain fronting
- FakeTLS implementation - by default appears as "storage.googleapis.com" domain name for standard ssl clients
- Further proxy chaining
- FireHOL blocklists

## To run

```shell
docker-compose build
docker-compose up
```

## Connect

Default params for connection are:
- Protocol: `Fake-TLS, URL-safe base64 secret`
- Server: `116.203.55.220`
- Port: `443`
- Hex secret: `7e673f88b044e516725dc2b71d21e8a9`
- Fake-TLS domain: `storage.googleapis.com`

Telegram connection link: `https://t.me/proxy?server=<YOUR_SERVER_IP>&port=443&secret=7n5nP4iwROUWcl3Ctx0h6KlzdG9yYWdlLmdvb2dsZWFwaXMuY29t`

## Generate secret

```shell
$ docker run --rm nineseconds/mtg:2 generate-secret google.com
7ibaERuTSGPH1RdztfYnN4tnb29nbGUuY29t

$ docker run --rm nineseconds/mtg:2 generate-secret --hex google.com
ee473ce5d4958eb5f968c87680a23854a0676f6f676c652e636f6d
```

Set new secret in `mtg.config.toml`

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
### TODO
Run all this as a single container

### More
- [Telegram MTProto Proxy link generator](http://seriyps.ru/mtpgen.html)

## Why should I use the Cloudflare hidden resolver?
[Cloudflare TOR hidden resolver](https://blog.cloudflare.com/welcome-hidden-resolver)

First and foremost, resolving DNS queries through the Tor network, for instance by connecting to Google’s 8.8.8.8 resolver, guarantees a significantly higher level of anonymity than making the requests directly. Not only does doing so prevent the resolver from ever seeing your IP address, even your ISP won’t know that you’ve attempted to resolve a domain name.

Still, unless the destination is an onion service, passive attackers can capture packets exiting the Tor network and malicious Exit Nodes can poison DNS queries or downgrade encryption through sslstripping. Even if you limit your browsing to only HTTPS sites, passive attackers can find out which addresses you’ve connected to. Even worse, actors capable of comparing traffic both before it enters the Tor network and after it leaves the network can potentially use the metadata (size, time, etc.) to deanonymize the client. The only solution, then, is to eliminate the need for Exit Nodes by using onion services instead. That is what our .onion-based resolver offers.

Moreover, if your client does not support encrypted DNS queries, using a .onion-based resolver can secure the connection from on-path attacks, including BGP hijacking attacks. This means having the same level of security for DNS-over-UDP and DNS-over-TCP as DNS-over-HTTPS and DNS-over-TLS provides.

Your personal anonymity, however, is not the only reason why you should use this service. The power of Tor in ensuring everyone’s anonymity rests on the number of people who use it. If only whistleblowers, for instance, were to use the Tor network, then anyone connecting to the Tor network would automatically be suspected of being a whistleblower. Therefore the more people use Tor to browse memes or to watch cat videos on the Internet, the easier it will be for those who truly need anonymity to blend in with the traffic.

One barrier to using Tor for many users is that it is simply slow, so I can try to sympathize with those who wouldn’t sacrifice quick website load times to help keep activists and dissidents anonymous. That said, DNS requests are small in size and since most browsers and operating systems cache DNS results the total traffic is not significant. As a result, using the .onion-based resolver will only slightly slow down your initial DNS request without slowing down anything else, while still contributing to the overall anonymity of the Tor network and its users.
