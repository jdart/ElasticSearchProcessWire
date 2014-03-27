# ElasticSearch for ProcessWire 0.1.0

## About ElasticSearch for ProcessWire

The ElasticSearch module for [ProcessWire CMS/CMF](http://processwire.com/) will add your page content to an ElasticSearch index, and give you a convenient way to search it.

* [Information about the author](http://metricmarketing.ca/jonathan-dart)
* [Information about Metric Marketing](http://metricmarketing.ca)
* [The author's blog](http://metricmarketing.ca/blog/author/jonathan-dart)
* [Learn more about ProcessWire](http://processwire.com)

## Installation

* Make sure PHP has the cURL library installed. (apt-get install php5-curl)
* Install [ElasticSearch](http://www.elasticsearch.org/overview/elkdownloads/).
* Install this module, the [usual methods](http://modules.processwire.com/install-uninstall/) apply.
* Once the module is installed, go to the module settings and configure it for your ElasticSearch instance, and click save. You should also click "Index All Pages".

When pages are saved ElasticSearch will be updated with the new content.

## Usage

    $pages = wire('modules')->get('ElasticSearch')->search('Something to search for'); 

## ElasticSearch Configuration

Configuring ElasticSearch can be very simple, if running ElasticSearch on a single server you really only need the below configurations (/etc/elasticsearch/elasticsearch.yml):

    index.number_of_shards: 1
    index.number_of_replicas: 0
    discovery.zen.ping.multicast.enabled: false
    discovery.zen.ping.unicast.hosts: []

## What gets searched?

I made a quick go of trying to get all content from a page, it's working for text fields, translated fields, repeaters and filenames.