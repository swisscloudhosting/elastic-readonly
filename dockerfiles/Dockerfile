FROM docker.elastic.co/elasticsearch/elasticsearch:7.10.1

RUN bin/elasticsearch-plugin install --batch ingest-attachment

# https://api.beshu.tech/download/es?esVersion=7.9.1
# Mirror for readonlyrest-1.23.1_es7.9.1.zip -> https://matterhorn.swisscloudhosting.ch/s/DcksMwF7zkrcbYW/download
RUN /usr/share/elasticsearch/bin/elasticsearch-plugin install -b "https://api.beshu.tech/download/es?esVersion=7.10.1"

# Configure Elasticsearch
COPY elasticsearch.yml /usr/share/elasticsearch/config/elasticsearch.yml
