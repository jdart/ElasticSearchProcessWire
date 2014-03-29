# ElasticSearch for ProcessWire 0.3.0

Since 0.3.0 ElasticSearch for ProcessWire is using the GPLv3 license.

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
* In the module settings configure your ElasticSearch IP and port, and click save.
* In the module settings click "Index All Pages".

## Usage

### Synchonizing Page Data with ElasticSearch

When pages are saved, deleted, trashed, and restored ElasticSearch will be notified accordinly, so after you've clicked "Index All Pages" you shouldn't need to think about it again.

## What gets searched?

Most content from a page should be seachable: text fields, translated fields, repeaters and filenames.

### Searching

The module has a function search() that returns a PageArray.

    $pages = $modules->get('ElasticSearch')->search('foo bar'); 

### Pagination

You can indicate ranges of results you're interested in and you can get a total number of results so you can generate pagination.

    $pages = $modules->get('ElasticSearch')->search('foo bar', $offset, $results_per_page); 

	$pages->getTotal();

### Minimum Scores

The search function also takes a 4th parameter to configure the minimum score required for a page to match. 

	$pages = $modules->get('ElasticSearch')->search('foo bar', $offset, $results_per_page, 0.05);

### Custom Queries

By default this module uses a [fuzzy_like_this](http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/query-dsl-flt-query.html#_how_it_works) query to match pages. To change that use an array for the 1st parameter when calling search.

    $pages = $modules->get('ElasticSearch')->search(array(
        'query' => array(
            'match' => array(
                '_all' => 'foo bar',
            ),
        ),
    ));

## ElasticSearch 

### Configuration

Configuring ElasticSearch can be very simple, if running ElasticSearch on a single server you really only need the below configuration in `/etc/elasticsearch/elasticsearch.yml`:

    index.number_of_shards: 1
    index.number_of_replicas: 0
    discovery.zen.ping.multicast.enabled: false
    discovery.zen.ping.unicast.hosts: []
    