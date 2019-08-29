FROM docker.elastic.co/elasticsearch/elasticsearch:7.2.0

RUN bin/elasticsearch-plugin install --batch ingest-attachment

# Mirror for readonlyrest-1.18.4_es7.2.0.zip -> https://matterhorn.swisscloudhosting.ch/s/zBg7kfdXYCrPFAk/download
#RUN /usr/share/elasticsearch/bin/elasticsearch-plugin install -b "https://m0ppfybs81.execute-api.eu-west-1.amazonaws.com/dev/download/es?esVersion=7.2.0&pluginVersion=1.18.4"
RUN /usr/share/elasticsearch/bin/elasticsearch-plugin install -b "https://matterhorn.swisscloudhosting.ch/s/zBg7kfdXYCrPFAk/download"

# Configure Elasticsearch
COPY elasticsearch.yml /usr/share/elasticsearch/config/elasticsearch.yml
