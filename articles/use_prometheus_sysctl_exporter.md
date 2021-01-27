---
title: "prometheus_sysctl_exporterを使用する"
emoji: "😸"
type: "tech"
topics: ["freebsd", "prometheus"]
published: true
---

## prometheus_sysctl_exporterについて

FreeBSD 12.0からベースにprometheus_sysctl_exporterというコマンドが追加されました。名前から分かる通り、カーネルの状態をprometheus向けに出力するコマンドです。

manを参照するとこのコマンドの結果をprometheusに取り込めるように出力する方法は2つあります。

1. cronから定期実行した結果をnode_exportのtextfile collectorを使用して出力する。
2. inetdから呼び出す。-hオプションを用いるとhttpヘッダーを同時に出力してくれるのでhttpサーバとして用いることができる。inetd.confにサンプルの設定が記載されていました。

今回は1つ目の方法を使用します。

## 設定内容

パッケージをインストールし、cronを設定すれば動作します。

### 必要なパッケージ

pkgを使用して2つパッケージをインストールします。1つはnode_exporter、もう一つはspongeを使用するためにmoreutilsをインストールします。

```sh
pkg install moreutils node_exporter
```

### cron

毎分prometheus_sysctl_exporterを実行し、spongeを経由してファイルを出力します。場所はパッケージ使用した場合のデフォルトのtext collectorの出力先です。拡張子が.promである必要があります。

```text
/usr/local/etc/cron.d/prom
*/1 *  *  *  *  root    prometheus_sysctl_exporter | sponge /var/tmp/node_exporter/sysctl_exporter.prom
```

## 参考資料

https://github.com/prometheus/node_exporter/tree/master/text_collector_examples
