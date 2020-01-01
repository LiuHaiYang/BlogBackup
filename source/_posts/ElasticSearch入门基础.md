---
title: ElasticSearch入门基础
tags: 大数据
abbrlink: 17678
date: 2018-08-12 15:50:03
---

### elasticsearch 
ElasticSearch是一个基于Lucene的搜索服务器。它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。

### cluster
代表一个集群，集群中有多个节点，其中有一个为主节点，这个主节点是可以通过选举产生的，主从节点是对于集群内部来说的。es的一个概念就是去中心化，字面上理解就是无中心节点，这是对于集群外部来说的，因为从外部来看es集群，在逻辑上是个整体，你与任何一个节点的通信和与整个es集群通信是等价的。

### shards
代表索引分片，es可以把一个完整的索引分成多个分片，这样的好处是可以把一个大的索引拆分成多个，分布到不同的节点上。构成分布式搜索。分片的数量只能在索引创建前指定，并且索引创建后不能更改。

### replicas
代表索引副本，es可以设置多个索引的副本，副本的作用一是提高系统的容错性，当某个节点某个分片损坏或丢失时可以从副本中恢复。二是提高es的查询效率，es会自动对搜索请求进行负载均衡。

### recovery
代表数据恢复或叫数据重新分布，es在有节点加入或退出时会根据机器的负载对索引分片进行重新分配，挂掉的节点重新启动时也会进行数据恢复。

### river
代表es的一个数据源，也是其它存储方式（如：数据库）同步数据到es的一个方法。它是以插件方式存在的一个es服务，通过读取river中的数据并把它索引到es中，官方的river有couchDB的，RabbitMQ的，Twitter的，Wikipedia的。

### gateway
代表es索引快照的存储方式，es默认是先把索引存放到内存中，当内存满了时再持久化到本地硬盘。gateway对索引快照进行存储，当这个es集群关闭再重新启动时就会从gateway中读取索引备份数据。es支持多种类型的gateway，有本地文件系统（默认），分布式文件系统，Hadoop的HDFS和amazon的s3云存储服务。

### discovery.zen
代表es的自动发现节点机制，es是一个基于p2p的系统，它先通过广播寻找存在的节点，再通过多播协议来进行节点之间的通信，同时也支持点对点的交互。

### Transport
代表es内部节点或集群与客户端的交互方式，默认内部是使用tcp协议进行交互，同时它支持http协议（json格式）、thrift、servlet、memcached、zeroMQ等的传输协议（通过插件方式集成）。

<hr/>
### py_es_eg1:

    from datetime import datetime
    from elasticsearch import Elasticsearch
 
    #连接elasticsearch,默认是9200
    es = Elasticsearch(host='192.168.1.129',port='9200')
     
    #创建索引，索引的名字是my-index,如果已经存在了，就返回个400，
    #这个索引可以现在创建，也可以在后面插入数据的时候再临时创建
    es.indices.create(index='my-index',ignore)
    #{u'acknowledged':True}
     
    #插入数据,(这里省略插入其他两条数据，后面用)
    es.index(index="my-index",doc_type="test-type",id=01,body={"any":"data01","timestamp":datetime.now()})
    
    #{u'_type':u'test-type',u'created':True,u'_shards':{u'successful':1,u'failed':0,u'total':2},u'_version':1,u'_index':u'my-index',u'_id':u'1}
    #也可以，在插入数据的时候再创建索引test-index
    es.index(index="test-index",doc_type="test-type",id=42,body={"any":"data","timestamp":datetime.now()})
     
    
    #查询数据，两种get and search
    #get获取
    res = es.get(index="my-index", doc_type="test-type", id=01)
    print(res)
     {u'_type': u'test-type', u'_source': {u'timestamp': u'2016-01-20T10:53:36.997000', u'any': u'data01'}, u'_index': u'my-index', u'_version': 1, u'found': True, u'_id': u'1'}
    print(res['_source'])
    #{u'timestamp': u'2016-01-20T10:53:36.997000', u'any': u'data01'}
     
    #search获取
    res = es.search(index="test-index", body={"query":{"match_all":{}}})
    print(res)
    '''{u'hits':
        {
        u'hits': [
            {u'_score': 1.0, u'_type': u'test-type', u'_id': u'2', u'_source': {u'timestamp': u'2016-01-20T10:53:58.562000', u'any': u'data02'}, u'_index': u'my-index'},
            {u'_score': 1.0, u'_type': u'test-type', u'_id': u'1', u'_source': {u'timestamp': u'2016-01-20T10:53:36.997000', u'any': u'data01'}, u'_index': u'my-index'},
            {u'_score': 1.0, u'_type': u'test-type', u'_id': u'3', u'_source': {u'timestamp': u'2016-01-20T11:09:19.403000', u'any': u'data033'}, u'_index': u'my-index'}
        ],
        u'total': 5,
        u'max_score': 1.0
        },
    u'_shards': {u'successful': 5, u'failed': 0, u'total':5},
    u'took': 1,
    u'timed_out': False
    }'''
    for hit in res['hits']['hits']:
        print(hit["_source"])
    res = es.search(index="test-index", body={'query':{'match':{'any':'data'}}}) #获取any=data的所有值
    print(res)
    

### py_es_eg2:
    
    from datetime import datetime   
    from elasticsearch import Elasticsearch
    # by default we connect to localhost:9200
    es =  Elasticsearch(host='192.168.1.129',port='9200')
    # datetimes will be serialized
    es.index(index="my-index", doc_type="test-type", id=42, body={"any": "data", "timestamp": datetime.now()})
    # {u'_id': u'42', u'_index': u'my-index', u'_type': u'test-type', u'_version': 1, u'ok': True}
    
    # but not deserialized
    a = es.get(index="my-index", doc_type="test-type", id=42)['_source']
    print(a)
    # {u'any': u'data', u'timestamp': u'2013-05-12T19:45:31.804229'}


### py_es_eg3:
    import traceback
    from pymongo import MongoClient
    from elasticsearch import Elasticsearch
    
    # 建立到MongoDB的连接
    _db = MongoClient('mongodb://127.0.0.1:27017')['blog']
    
    # 建立到Elasticsearch的连接
    _es = Elasticsearch()
    
    # 初始化索引的Mappings设置
    _index_mappings = {
      "mappings": {
        "user": { 
          "properties": { 
            "title":    { "type": "text"  }, 
            "name":     { "type": "text"  }, 
            "age":      { "type": "integer" }  
          }
        },
        "blogpost": { 
          "properties": { 
            "title":    { "type": "text"  }, 
            "body":     { "type": "text"  }, 
            "user_id":  {
              "type":   "keyword" 
            },
            "created":  {
              "type":   "date"
            }
          }
        }
      }
    }
    
    # 如果索引不存在，则创建索引
    if _es.indices.exists(index='blog_index') is not True:
      _es.indices.create(index='blog_index', body=_index_mappings) 
    
    # 从MongoDB中查询数据，由于在Elasticsearch使用自动生成_id，因此从MongoDB查询
    # 返回的结果中将_id去掉。
    user_cursor = db.user.find({}, projection={'_id':False})
    user_docs = [x for x in user_cursor]
    
    # 记录处理的文档数
    processed = 0
    # 将查询出的文档添加到Elasticsearch中
    for _doc in user_docs:
      try:
        # 将refresh设为true，使得添加的文档可以立即搜索到；
        # 默认为false，可能会导致下面的search没有结果
        _es.index(index='blog_index', doc_type='user', refresh=True, body=_doc)
        processed += 1
        print('Processed: ' + str(processed), flush=True)
      except:
        traceback.print_exc()
    
    # 查询所有记录结果
    print('Search all...',  flush=True)
    _query_all = {
      'query': {
        'match_all': {}
      }
    }
    _searched = _es.search(index='blog_index', doc_type='user', body=_query_all)
    print(_searched, flush=True)
    
    # 输出查询到的结果
    for hit in _searched['hits']['hits']:
      print(hit['_source'], flush=True)
    
    # 查询姓名中包含jerry的记录
    print('Search name contains jerry.', flush=True)
    _query_name_contains = {
      'query': {
        'match': {
          'name': 'jerry'
        }
      }
    }
    _searched = _es.search(index='blog_index', doc_type='user', body=_query_name_contains)
    print(_searched, flush=True)