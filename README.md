# elk
Scope: Using 3 different Docker images (official Elastic docker images) 
- Step 1: Setup Elasticsearch container and verify elastic its working 
docker-machine ssh 

sudo sysctl -w vm.max_map_count=262144

docker run -d -p 9200:9200 -p 9300:9300 -it -h elasticsearch --name elasticsearch elasticsearch 

curl http://localhost:9200/ 

- Step 2: Setup Kibana container https://www.elastic.co/guide/en/logst... 

docker run -d -p 5601:5601 -h kibana --name kibana --link elasticsearch:elasticsearch kibana 

curl http://localhost:9200/_cat/indices 

- Step 3: Create logstash config file and use it to setup Logstash container 

docker run -h logstash --name logstash --link elasticsearch:elasticsearch -it --rm -v "$PWD":/config-dir logstash -f /config-dir/logstash.conf 

curl http://localhost:9200/_cat/indices 

docker run -d -p 9500:9500 -h logstash2 --name logstash2 --link elasticsearch:elasticsearch --rm -v "$PWD":/config-dir logstash -f /config-dir/logstash2.conf
