---
title: "MACアドレスのランダム化によりMACアドレスは衝突するのか"
emoji: "😸"
type: "tech"
topics: ["ios", "android", "macaddress"]
published: true
---

## 初めに

iOSが14からデフォルトでWi-FiのMACアドレスをランダム化するようになりました。そのせいで
ルーターとアドレスが衝突したという噂を聞いたので有り得るのか調べてみました。

結論としては、まず衝突は有り得ません。

## ランダム化されたMACアドレスについて

CiscoなどのベンダーによるとiOSやAndroidが使用するランダムなMACアドレスは以下の通りです。

```
x2:xx:xx:xx:xx:xx
x6:xx:xx:xx:xx:xx
xA:xx:xx:xx:xx:xx
xE:xx:xx:xx:xx:xx
```

このアドレスは第1オクテットの下位2ビットが10になっています。この2ビットはローカルアドレスであり、ユニキャストアドレスであることを示します。OUIとしてネットワーク機器を製造するベンダーに割り当てているアドレスは2ビットが00となります。よってルーターとアドレスが衝突することはありません。ランダム化されたアドレスどうしの衝突も$2^{48}$のパターンがあるため、まず心配しなくて大丈夫でしょう。

また以下のパターンも考えられますが、一般的な利用では衝突が発生する可能性は低いと思います。

* 歴史的経緯でIEEEがルールを定める前のMACアドレスを使用している場合。
* CIDを取得しており、iOSなどとネットワークを共有している場合。

## 参考資料

* [RFC7042](https://tools.ietf.org/html/rfc7042)
* [MACアドレスのローカル、グローバルの判定方法](https://teratail.com/questions/246724)
* [EthernetでのID「MACアドレス」を理解する](https://ascii.jp/elem/000/000/417/417556/)
* [IEEE SA - Registration Authority](https://standards.ieee.org/products-services/regauth/index.html)
* [MAC address](https://en.wikipedia.org/wiki/MAC_address)
* [Field Notice: FN - 70610 - Cisco Identity Services Engine MAC Address Lookup Might Fail with Android 10, Android 11, and Apple iOS 14 Devices Due to the Use of MAC Randomization on the Mobile Client Devices - Workaround Provided](https://www.cisco.com/c/en/us/support/docs/field-notices/706/fn70610.html)
* [Meraki and iOS 14 MAC Address Randomization](https://documentation.meraki.com/General_Administration/Cross-Platform_Content/Meraki_and_iOS_14_MAC_Address_Randomization)
