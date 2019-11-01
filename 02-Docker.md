# Docker

![Docker graphic](images/Docker-R-Logo-08-2018-Monochomatic-RGB_Moby-x1.png)

The steps to follow to install Elasticsearch with Docker.

## Contents

The content are as follows:

* [Dockerized Elasticsearch](#dockerized-elasticsearch)
    * [Running Elasticsearch](#running-elasticsearch)
    * [Verifying Elasticsearch](#verifying-elasticsearch)
* [Dockerized Kibana](#dockerized-kibana)
    * [Running Kibana](#running-kibana)
    * [Verifying Kibana](#verifying-kibana)

## Dockerized Elasticsearch

Let's see what Docker images there are:

```bash
$ docker search elasticsearch
NAME                                 DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
elasticsearch                        Elasticsearch is a powerful open source sear…   3830                [OK]
nshou/elasticsearch-kibana           Elasticsearch-7.1.1 Kibana-7.1.1                105                                     [OK]
itzg/elasticsearch                   Provides an easily configurable Elasticsearc…   68                                      [OK]
mobz/elasticsearch-head              elasticsearch-head front-end and standalone …   48
elastichq/elasticsearch-hq           Official Docker image for ElasticHQ: Elastic…   36                                      [OK]
elastic/elasticsearch                The Elasticsearch Docker image maintained by…   22
taskrabbit/elasticsearch-dump        Import and export tools for elasticsearch       18                                      [OK]
bitnami/elasticsearch                Bitnami Docker Image for Elasticsearch          18                                      [OK]
lmenezes/elasticsearch-kopf          elasticsearch kopf                              18                                      [OK]
barnybug/elasticsearch               Latest Elasticsearch 1.7.2 and previous rele…   17                                      [OK]
esystemstech/elasticsearch           Debian based Elasticsearch packing for Lifer…   15
monsantoco/elasticsearch             ElasticSearch Docker image                      11                                      [OK]
mesoscloud/elasticsearch             [UNMAINTAINED] Elasticsearch                    9                                       [OK]
justwatch/elasticsearch_exporter     Elasticsearch stats exporter for Prometheus     9
blacktop/elasticsearch               Alpine Linux based Elasticsearch Docker Image   8                                       [OK]
centerforopenscience/elasticsearch   Elasticsearch                                   4                                       [OK]
barchart/elasticsearch-aws           Elasticsearch AWS node                          3
dtagdevsec/elasticsearch             elasticsearch                                   3                                       [OK]
bitnami/elasticsearch-exporter       Bitnami Elasticsearch Exporter Docker Image     2                                       [OK]
phenompeople/elasticsearch           Elasticsearch is a powerful open source sear…   1                                       [OK]
jetstack/elasticsearch-pet           An elasticsearch image for kubernetes PetSets   1                                       [OK]
18fgsa/elasticsearch-ha              Built from https://github.com/18F/kubernetes…   0
axway/elasticsearch-docker-beat      "Beat" extension to read logs of containers …   0                                       [OK]
18fgsa/elasticsearch                 Built from https://github.com/docker-library…   0
wreulicke/elasticsearch              elasticsearch                                   0                                       [OK]
$
```

Okay, we will try the official release. Instructions are here:

    http://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html

Apache 2.0 licensed versions are here:

    http://www.docker.elastic.co/

[Note that Elastic do not use the `latest` tag (which seems like a good practice) so a __tagged__ release must be installed.]

```bash
$ docker pull docker.elastic.co/elasticsearch/elasticsearch:7.3.1
7.3.1: Pulling from elasticsearch/elasticsearch
48914619bcd3: Pull complete
c00e9e7aed99: Pull complete
bfac725fd799: Pull complete
f1fe76a8ef9b: Pull complete
2a1b75592794: Pull complete
f33ae02b8315: Pull complete
ad8bc430d6cc: Pull complete
Digest: sha256:d43c5b93c027fbbfc5b24a69a786f66d6d4059360d8c1b9defb661f68dc24c74
Status: Downloaded newer image for docker.elastic.co/elasticsearch/elasticsearch:7.3.1
docker.elastic.co/elasticsearch/elasticsearch:7.3.1
$
```

Note that the virtual memory limit must be raised. On linux this is as follows:

```bash
$ sudo sysctl -w vm.max_map_count=262144
[sudo] password for xxxxx:
vm.max_map_count = 262144
$
```

#### Running Elasticsearch

Run the docker image:

```bash
$ docker run -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" --name es7.3.1 --rm docker.elastic.co/elasticsearch/elasticsearch:7.3.1
...
$
```

As this is for development testing and use, we are specifying
[Single-node discovery](https://www.elastic.co/guide/en/elasticsearch/reference/current/bootstrap-checks.html#single-node-discovery)
which disables [Bootstrap Checks](http://www.elastic.co/guide/en/elasticsearch/reference/current/bootstrap-checks.html).

[As usual, Ctrl-C to stop.]

#### Verifying Elasticsearch

If we go to http://localhost:9200 in a browser, we should get:

```
{
  "name" : "be637d19e825",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "7urUewFVT_i3pf7eCB1ssQ",
  "version" : {
    "number" : "7.3.1",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "4749ba6",
    "build_date" : "2019-08-19T20:19:25.651794Z",
    "build_snapshot" : false,
    "lucene_version" : "8.1.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

## Dockerized Kibana

It looks like we don't get Kibana this way, so let's get that:

```bash
$ docker pull docker.elastic.co/kibana/kibana:7.3.1
...
$
```

[It's probably not critical that the Kibana version should match the Elasticsearch version.]

The instructions are here:

    http://www.elastic.co/guide/en/kibana/7.3/docker.html

#### Running Kibana

To run Kibana:

```bash
$ docker run --link es7.3.1:elasticsearch -p 5601:5601 --name kibana7.3.1 --rm docker.elastic.co/kibana/kibana:7.3.1
...
$
```

[As usual, Ctrl-C to stop.]

#### Verifying Kibana

If we go to http://localhost:5601 in a browser, we should see a Kibana console.

If things are not synchonized yet, there will be a __Kibana server is not ready yet__ message.
So wait a few seconds and try again.

[Return to main Elasticsearch page](README.md#version)
