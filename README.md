# docker-elk-stack
Addition to "deviantony/docker-elk", has Filebeat added on to it and is configured with ElasticPhish.

# Installing ElasticPhish
To install ElasticPhis the following docker command can be executed :

``docker run -d --cap-add=NET_ADMIN -p 4000:4000 --net docker-elk_elk --name calidog --hostname calidog lab8``.

# Changes made to default configurations
## Filebeat
Under Filebeat I have added the following lines 
```
        - type: bind
        source: /home/asambhav/Lab8/ElasticPhish/phishing_domains.txt
        target: /usr/share/filebeat/domains/phishing_domains.txt
        read_only: true
```
I added the following lines, this just made use that the “phishing_domains.txt” was binded onto the container. Then I later executed the configuration using the “command” field.

```    command: filebeat -e -c /usr/share/filebeat/filebeat.yml```

### Filebeat Config
Specified the path of the file to read inside the container 
```
  paths:
    - /usr/share/filebeat/domains/phishing_domains.txt
```
I also setup output to be our logstash container 
```
output.logstash:
  # The Logstash hosts
  hosts: ["logstash:5044"]
  ```
## Logstash
I have multiple configs with "data" directory but I am making use of "pipelines" to specify which configs to be ran on-boot.
