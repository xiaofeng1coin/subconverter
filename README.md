# subconverter

Utility to convert between various proxy subscription formats.

[![Build Status](https://github.com/asdlokj1qpi23/subconverter/actions/workflows/docker.yml/badge.svg)](https://github.com/asdlokj1qpi23/subconverter/actions)
[![GitHub tag (latest SemVer)](https://img.shields.io/github/tag/asdlokj1qpi23/subconverter.svg)](https://github.com/asdlokj1qpi23/subconverter/tags)
[![GitHub release](https://img.shields.io/github/release/asdlokj1qpi23/subconverter.svg)](https://github.com/asdlokj1qpi23/subconverter/releases)
[![GitHub license](https://img.shields.io/github/license/asdlokj1qpi23/subconverter.svg)](https://github.com/tindy2013/subconverter/blob/master/LICENSE)

[Docker README](https://github.com/asdlokj1qpi23/subconverter/blob/master/README-docker.md)

[中文文档](https://github.com/asdlokj1qpi23/subconverter/blob/master/README-cn.md)

- [subconverter](#subconverter)
  - [Docker](#docker)
  - [Supported Types](#supported-types)
  - [Quick Usage](#quick-usage)
    - [Access Interface](#access-interface)
    - [Description](#description)
  - [Advanced Usage](#advanced-usage)
  - [Auto Upload](#auto-upload)
  
## Docker

For running this docker, simply use the following commands:
```bash
# run the container detached, forward internal port 25500 to host port 25500
docker run -d --restart=always -p 25500:25500 asdlokj1qpi23/subconverter:latest
# then check its status
curl http://localhost:25500/version
# if you see `subconverter vx.x.x backend` then the container is up and running
```
Or run in docker-compose:
```yaml
---
version: '3'
services:
  subconverter:
    image: asdlokj1qpi23/subconverter:latest
    container_name: subconverter
    ports:
      - "15051:25500"
    restart: always
```
## Supported Types

| Type                              | As Source | As Target    | Target Name    |
|-----------------------------------|:---------:| :----------: |----------------|
| Clash                             |     ✓     |      ✓       | clash          |
| ClashR                            |     ✓     |      ✓       | clashr         |
| Quantumult                        |     ✓     |      ✓       | quan           |
| Quantumult X                      |     ✓     |      ✓       | quanx          |
| Loon                              |     ✓     |      ✓       | loon           |
| SS (SIP002)                       |     ✓     |      ✓       | ss             |
| SS Android                        |     ✓     |      ✓       | sssub          |
| SSD                               |     ✓     |      ✓       | ssd            |
| SSR                               |     ✓     |      ✓       | ssr            |
| Surfboard                         |     ✓     |      ✓       | surfboard      |
| Surge 2                           |     ✓     |      ✓       | surge&ver=2    |
| Surge 3                           |     ✓     |      ✓       | surge&ver=3    |
| Surge 4                           |     ✓     |      ✓       | surge&ver=4    |
| V2Ray                             |     ✓     |      ✓       | v2ray          |
| Telegram-liked HTTP/Socks 5 links |     ✓     |      ×       | Only as source |
| Singbox                           |     ✓      |      ✓       | singbox        |

Notice:

1. Shadowrocket users should use `ss`, `ssr` or `v2ray` as target.

2. You can add `&remark=` to Telegram-liked HTTP/Socks 5 links to set a remark for this node. For example:

   - tg://http?server=1.2.3.4&port=233&user=user&pass=pass&remark=Example

   - https://t.me/http?server=1.2.3.4&port=233&user=user&pass=pass&remark=Example


---

## Quick Usage

> Using default groups and rulesets configuration directly, without changing any settings

### Access Interface

```txt
http://127.0.0.1:25500/sub?target=%TARGET%&url=%URL%&config=%CONFIG%
```

### Description

| Argument | Required | Example | Description |
| -------- | :------: | :------ | ----------- |
| target   | Yes      | clash   | Target subscription type. Acquire from Target Name in [Supported Types](#supported-types). |
| url      | Yes      | https%3A%2F%2Fwww.xxx.com | Subscription to convert. Supports URLs and file paths. Process with [URLEncode](https://www.urlencoder.org/) first. |
| config   | No       | https%3A%2F%2Fwww.xxx.com | External configuration file path. Supports URLs and file paths. Process with [URLEncode](https://www.urlencoder.org/) first. More examples can be found in [this](https://github.com/lzdnico/subconverteriniexample) repository. |

If you need to merge two or more subscription, you should join them with '|' before the URLEncode process.

Example:

```txt
You have 2 subscriptions and you want to merge them and generate a Clash subscription:
1. https://dler.cloud/subscribe/ABCDE?clash=vmess
2. https://rich.cloud/subscribe/ABCDE?clash=vmess

First use '|' to separate 2 subscriptions:
https://dler.cloud/subscribe/ABCDE?clash=vmess|https://rich.cloud/subscribe/ABCDE?clash=vmess

Then process it with URLEncode to get %URL%:
https%3A%2F%2Fdler.cloud%2Fsubscribe%2FABCDE%3Fclash%3Dvmess%7Chttps%3A%2F%2Frich.cloud%2Fsubscribe%2FABCDE%3Fclash%3Dvmess

Then fill %TARGET% and %URL% in Access Interface with actual values:
http://127.0.0.1:25500/sub?target=clash&url=https%3A%2F%2Fdler.cloud%2Fsubscribe%2FABCDE%3Fclash%3Dvmess%7Chttps%3A%2F%2Frich.cloud%2Fsubscribe%2FABCDE%3Fclash%3Dvmess

Finally subscribe this link in Clash and you are done!
```

---

## Advanced Usage

Please refer to [中文文档](https://github.com/asdlokj1qpi23/subconverter/blob/master/README-cn.md#%E8%BF%9B%E9%98%B6%E7%94%A8%E6%B3%95).

## Auto Upload

> Upload Gist automatically

Add a [Personal Access Token](https://github.com/settings/tokens/new) into [gistconf.ini](./gistconf.ini) in the root directory, then add `&upload=true` to the local subscription link, then when you access this link, the program will automatically update the content to Gist repository.

Example:

```ini
[common]
;uncomment the following line and enter your token to enable upload function
token = xxxxxxxxxxxxxxxxxxxxxxxx(Your Personal Access Token)
```
## Thanks
[tindy2013](https://github.com/tindy2013)
[https://github.com/tindy2013/subconverter](https://github.com/tindy2013/subconverter)