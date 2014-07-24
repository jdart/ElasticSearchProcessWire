# ElasticSearch for ProcessWire 0.4.4

## About ElasticSearch for ProcessWire

The ElasticSearch module for [ProcessWire CMS/CMF](http://processwire.com/) will add your page content to an ElasticSearch index per site, and give you a convenient way to search it.

* [Information about the author](http://metricmarketing.ca/jonathan-dart)
* [Information about Metric Marketing](http://metricmarketing.ca)
* [The author's blog](http://metricmarketing.ca/blog/author/jonathan-dart)
* [Learn more about ProcessWire](http://processwire.com)

## Installation

* Make sure the PHP cURL library is installed. (apt-get install php5-curl)
* Install [ElasticSearch](http://www.elasticsearch.org/overview/elkdownloads/).
* Install this module, the [usual methods](http://modules.processwire.com/install-uninstall/) apply.
* In the module settings configure your ElasticSearch IP and port, and click save.
* In the module settings click "Index All Pages".

## Usage

### Synchonizing Page Data with ElasticSearch

When pages are saved, deleted, trashed, and restored ElasticSearch will be notified accordingly, so after you've clicked "Index All Pages" you shouldn't need to think about it again.

## What gets searched?

Most content from a page should be seachable: text fields, translated fields, repeaters and filenames.

### Searching

This module has a function search() that returns a PageArray.

    $search_results = $modules->get('ElasticSearch')->search('foo bar'); 

### Pagination

The [MarkupPagerNav](http://processwire.com/api/modules/markup-pager-nav/) module should work out of the box.

    $search_results = $modules->get('ElasticSearch')->search('foo bar', $results_per_page); 

    echo "Total results: " . $search_results->getTotal();

	echo $search_results->renderPagination();

### Minimum Scores

The search function also takes a 3rd parameter to configure the minimum score required for a page to match. 

	$search_results = $modules->get('ElasticSearch')->search('foo bar', $results_per_page, 0.05);

### Custom Queries

By default this module uses a [fuzzy_like_this](http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/query-dsl-flt-query.html#_how_it_works) query to match pages. To change that use an array for the 1st parameter when calling search.

    $search_results = $modules->get('ElasticSearch')->search(array(
        'query' => array(
            'match' => array(
                '_all' => 'foo bar',
            ),
        ),
    ));

## ElasticSearch 

### Configuration

This plugin will create an index based on the http host, so if you have many sites one one server they can all safely use the same ElasticSearch server, no special configuration is required to support multiple sites.

Configuring ElasticSearch can be very simple, if running ElasticSearch on a single server you really only need the below configuration in `/etc/elasticsearch/elasticsearch.yml`:

    index.number_of_shards: 1
    index.number_of_replicas: 0
    discovery.zen.ping.multicast.enabled: false
    discovery.zen.ping.unicast.hosts: []
