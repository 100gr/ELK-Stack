# ELK Stack : INSTALLATION on Ubuntu 20.04.4 LTS

# CONFIGURE REPO


<br>

```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
sudo apt-get install apt-transport-https
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list
sudo apt-get update
```

------------------------------------------------------------

# ELASTICSEARCH : INSTALLATION


<br>

```
sudo apt-get install elasticsearch
sudo systemctl status elasticsearch.service
# edit Xmx and Xms /etc/elasticsearch/jvm.options
sudo systemctl start elasticsearch.service
sudo systemctl enable elasticsearch.service
curl 127.0.0.1:9200
```

* Cluster Settings

```
hostnamectl set-hostname global-linux-es1
vim /etc/elasticsearch/elasticsearch.yml

cluster.name: global-monitoring
node.name: global-linux-es1
# By default Elasticsearch is only accessible on localhost. Set a different address here to expose this node on the network:
network.host: 192.168.0.10
cluster.initial_master_nodes: ["global-linux-es1"]

curl 192.168.0.10:9200 # localhost will not work
```

------------------------------------------------------------

# KIBANA : INSTALLATION


<br>

```
sudo apt-get install kibana
sudo systemctl status kibana.service
# change server.host to "0.0.0.0" or "external ip"
sudo systemctl start kibana.service
sudo systemctl enable kibana.service
```

------------------------------------------------------------

# LOGSTASH : INSTALLATION


<br>

```
sudo apt-get install logstash
sudo systemctl status logstash.service
sudo systemctl start logstash.service
sudo systemctl enable logstash.service
```

* Cluster Settings

```
# Logstash srv
hostnamectl set-hostname global-linux-ls1

# Input, Fileters & Output; as minimum config we need only Input & Output
# Look for data from standard input (console): input { stdin { } }
# Output use elastic search pluging to sent data to: { elasticsearch { hosts => ["192.168.0.10"] }
/usr/share/logstash/bin/logstash -e 'input { stdin { } } output { elasticsearch { hosts => ["192.168.0.10"] } }'
# Type some text: TestTest 

# Elasticsearch srv
# To check if data made it
curl http://192.168.0.10.:9200/logstash-*/_search
# By default, Logstash will create indices, that is, where the data is sgtored, using the format "logstash-" and then the data.
# '*' ignore the date and search any indices that match
# _search - empty search to match all hits



```

* Returned result it hard to parse visually

```
sudo apt install jq
curl http://192.168.0.10:9200/logstash-*/_search | jq .

{
  "took": 0,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 1,
      "relation": "eq"
    },
    "max_score": 1,
    "hits": [
      {
        "_index": "logstash-2022.03.27-000001",
        "_type": "_doc",
        "_id": "P1qvy38BrDF8C7gg_i7L",
        "_score": 1,
        "_source": {
          "host": "global-linux-ls1",
          "message": "TestTest",
          "@version": "1",
          "@timestamp": "2022-03-27T14:03:03.102Z"
        }
      }
    ]
  }
}
```