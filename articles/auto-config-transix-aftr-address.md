---
title: "transixサービスのAFTRを仕様に従い取得してみる"
emoji: "🎉"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["transix", "dslite", "IPoE", "IPv6"]
published: true
---

DS-Liteの設定情報を取得する仕様を見つけたので、インターネットマルチフィードが提供するtransixサービスで試してみました。

* [v6mig-prov/spec.md at 1.1 · v6pc/v6mig-prov](https://github.com/v6pc/v6mig-prov/blob/1.1/spec.md)
* [ASAHIネットのDS-Liteの終端(AFTR)を取得するメモ](https://gist.github.com/stkchp/4daea9158439c32d7a70a255d51e568b)

```
$ dig 4over6.info txt +short @2404:1a8:7f01:b::3
"v=v6mig-1 url=https://setup46.transix.jp/config t=b"

$ curl -k "https://setup46.transix.jp/config?vendorid=acde48-v6pc_swg_hgw&product=V6MIG-ROUTER&version=0_00&capability=map_e,dslite,lw4o6,hubspoke"
{
  "enabler_name": "Internet Multifeed Co.",
  "service_name": "transix IPv4接続（DS-Lite）",
  "isp_name": "transix",
  "ttl": 86400,
  "order": [
    "dslite"
  ],
  "dslite": {
    "aftr": "2404:8e00::feed:101"
  }
}
```

transixのAFTRはFQDNが公開されていますが、IPv6アドレスが直に返されますね。

```
$ dig gw.transix.jp aaaa +short
2404:8e00::feed:102
2404:8e00::feed:100
2404:8e00::feed:101
```

4over6.infoのドメイン連絡先がお名前comのプライベートサービスだったり、仕様がどこで公開されているのか等が不透明ですね。
