安装JDK

install:
  brew install elasticsearch

Data:    /usr/local/var/elasticsearch/elasticsearch_bing/
Logs:    /usr/local/var/log/elasticsearch/elasticsearch_bing.log
Plugins: /usr/local/Cellar/elasticsearch/2.0.0_1/libexec/plugins/
Bin:     /usr/local/Cellar/elasticsearch/2.0.0_1/bin
Config:  /usr/local/etc/elasticsearch/
         /usr/local/Cellar/elasticsearch/2.0.0_1/libexec/config

To have launchd start elasticsearch at login:
  ln -sfv /usr/local/opt/elasticsearch/*.plist ~/Library/LaunchAgents
Then to load elasticsearch now:
  launchctl load ~/Library/LaunchAgents/homebrew.mxcl.elasticsearch.plist
Or, if you don't want/need launchctl, you can just run:
  elasticsearch

plugin:
  elasticsearch head (目前只支持1.7)
    a web front end for browsing and interacting with an Elastic Search cluster.

    ./plugin install mobz/elasticsearch-head

    open http://localhost:9200/_plugin/head/

    elasticsearch 1.7
    Data:    /usr/local/var/elasticsearch/elasticsearch_bing/
    Logs:    /usr/local/var/log/elasticsearch/elasticsearch_bing.log
    Plugins: /usr/local/var/lib/elasticsearch/plugins/
    Config:  /usr/local/etc/elasticsearch/
    /usr/local/opt/elasticsearch17/config/elasticsearch.yml


  Marvel | Monitor Elasticsearch
  需要安装 kibana 查看数据

Index:
  Movie.__elasticsearch__.create_index! force: true
  #force: ture当重新定义mapping后，需要强制索引

  Move.import

  Delete index: curl -XDELETE 'http://localhost:9200/movies/'

  检查状态：
    curl -XGET 'http://localhost:9200/_cluster/health?pretty=true'

  动态更改索引
    curl -XPUT 'localhost:9200/*/_settings' -d ' { "index" : { "number_of_replicas" : 0 } } '

  Check the mapping for an index:

    curl -XGET 'http://127.0.0.1:9200/products_development/_mapping?pretty=1'

  Get some sample docs:

    curl -XGET 'http://127.0.0.1:9200/products_development/_search?pretty=1'

