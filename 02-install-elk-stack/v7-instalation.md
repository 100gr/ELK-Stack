# ELK Stack : INSTALLATION on Ubuntu 20.04.4 LTS

<br>

```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
sudo apt-get install apt-transport-https
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list
sudo apt-get update
sudo apt-get install elasticsearch
sudo systemctl status elasticsearch.service
# edit Xmx and Xms /etc/elasticsearch/jvm.options
sudo systemctl start elasticsearch.service
sudo systemctl enable elasticsearch.service
curl 127.0.0.1:9200

# Cluster Settings
hostnamectl set-hostname global-linux-es1
vim /etc/elasticsearch/elasticsearch.yml

cluster.name: global-monitoring
node.name: global-linux-es1
# By default Elasticsearch is only accessible on localhost. Set a different 
# address here to expose this node on the network:
network.host: 192.168.0.10
cluster.initial_master_nodes: ["global-linux-es1"]

curl 192.168.0.10:9200 # local host will not work
hostnamectl set-hostname global-linux-es1

```

------------------------------------------------------------

# 02 ELK Stack : INSTALLATION


<br>

* install of kibana

```
sudo apt-get install kibana
sudo systemctl status kibana.service
# change server.host to "0.0.0.0" or "external ip"
sudo systemctl start kibana.service
sudo systemctl enable kibana.service
```

------------------------------------------------------------

# 02 ELK Stack : INSTALLATION

<br>

* install of logstash

```
sudo apt-get install logstash
sudo systemctl status logstash.service
sudo systemctl start logstash.service
sudo systemctl enable logstash.service
```
