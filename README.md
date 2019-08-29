# Docker image for Elasticsearch with Ingest Attachment Processor and ReadonlyREST Free to be used with nextcloud


## Purpose
The Nextcloud app Fulltext Search allows for fulltext search of files and bookmarks via Elasticsearch.

Elastic Search is accessed through an URL. Full access is possible, without additional authentication and authorization.

The ReadonlyREST plugin offers an approach to secure Elastic Search with username / password credentials. The core of this plugin is an ACL engine that checks each incoming request through a sequence of rules a bit like a firewall. There is a dozen rules that can be grouped in sequences of blocks and form a powerful representation of a logic chain.

## Basic authentication
 
 - Add basic authentication. In `readonlyrest.yml`:

```
readonlyrest:
  access_control_rules:

  - name: "Require HTTP Basic Auth"
    type: allow
    hosts: [localhost]
    auth_key: user:password 
```

or

```
readonlyrest:
  access_control_rules:
  - name: Accept requests from users in group team1 on index1
    groups: ["team1"]
    indices: ["index1"]

  users:
  - username: username
    auth_key_sha256: 280ac6f...94bf9
    groups: ["team1"]
```
The value is a string like `username:password` hashed in SHA256. 

 - Start the elasticsearch swarm container like this:

```
docker service create type=bind,src=/readonlyrest.yml,dst=/usr/share/elasticsearch/config/readonlyrest.yml swisscloudhosting/elastic-readonly:latest
```

## Disable X-Pack security module

(applies to ES 6.4.0 or greater)

ReadonlyREST and X-Pack security module can't run together, so the latter needs to be disabled.

Edit `elasticsearch.yml` and append `xpack.security.enabled: false`.

Or start the swarm COntainer like this:

```
docker service create --env xpack.security.enabled=false --env discovery.type=single-node swisscloudhosting/elastic-readonly:latest
```


## Test everything is working

The following command should succeed, and the response should show a status code 200.

```
curl -vvv -u user:password "http://localhost:9200/_cat/indices"
```

The following command should not succeed, and the response should show a status code 401

```
curl -vvv "http://localhost:9200/_cat/indices"

```
