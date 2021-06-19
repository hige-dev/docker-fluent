## nginx起動


```
docker run --name nginx1 -d --log-driver=fluentd -p 80:80 \
--log-opt fluentd-address=localhost:24224 \
--log-opt tag=docker.{{.Name}} nginx:latest
```