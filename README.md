## やりたかったこと

NginxのログをFluentdに送って、ログをparseした状態でElasticsearchに送る

### docker-composeでEKF起動

docker-compose up -d

### nginx起動

```sh
docker run --name nginx1 -d --log-driver=fluentd -p 80:80 \
--log-opt fluentd-address=localhost:24224 \
--log-opt tag=docker.{{.Name}} nginx:latest
```

## できなかったこと
fluentularでparseできたが、Fluentdでparseできなかった

### Fluentdに届いたNginxのログ

```sh
# 生ログと違い、ダブルクオートにエスケープが入る
192.168.80.1 - - [02/Jul/2021:06:23:45 +0000] \"GET / HTTP/1.1\" 200 612 \"-\" \"curl/7.29.0\" \"-\"
```

### fluentularではparseできた正規表現

```sh
^(?<remote>[^ ]*) (?<host>[^ ]*) (?<user>[^ ]*) \[(?<time>[^\]]*)\] \\"(?<method>\S+)(?: +(?<path>[^\"]*?)(?: +\S*)?)?" (?<code>[^ ]*) (?<size>[^ ]*) \\"(?:(?<referer>[^\"]*))\\" \\"(?<agent>[^"]*)\\" \\"(?:(?<x_forwarded_for>[^ ]+))\\"?$
```


