# 第 25 章 Elasticsearch API

```

<?xml version="1.0"?>
<project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"  >
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>cn.netkiller</groupId>
		<artifactId>example</artifactId>
		<version>0.0.1-SNAPSHOT</version>
	</parent>
	<groupId>cn.netkiller</groupId>
	<artifactId>elastic</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>elastic</name>
	<url>http://maven.apache.org</url>
	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	</properties>
	<dependencies>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>3.8.1</version>
			<scope>test</scope>
		</dependency>

		<dependency>
			<groupId>org.elasticsearch</groupId>
			<artifactId>elasticsearch</artifactId>
			<version>5.6.3</version>
		</dependency>
		<dependency>
			<groupId>org.elasticsearch.client</groupId>
			<artifactId>transport</artifactId>
			<version>5.6.3</version>
		</dependency>
		<dependency>
			<groupId>org.apache.logging.log4j</groupId>
			<artifactId>log4j-api</artifactId>
			<version>2.8.2</version>
		</dependency>
		<dependency>
			<groupId>org.apache.logging.log4j</groupId>
			<artifactId>log4j-core</artifactId>
			<version>2.8.2</version>
		</dependency>

	</dependencies>
</project>

```

## Client

```

Settings settings = Settings.builder()
					.put("cluster.name", "elasticsearch") //集群名称
					.put("client.transport.sniff", true) //自动嗅探
					.put("discovery.type", "zen")
					.put("discovery.zen.minimum_master_nodes", 1)
					.put("discovery.zen.ping_timeout", "500ms")
					.put("discovery.initial_state_timeout", "500ms")
					.build();
Client client = new PreBuiltTransportClient(settings)	.addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName(ip), 9300));

```

## insert

```
import org.elasticsearch.action.index.IndexResponse;  
import org.elasticsearch.client.Client;  

IndexResponse response = client.prepareIndex("information", "news", "1")  
.setSource("{ \"title\": \"ElasticSearch 5.5.2\"}")  
.get();  

```

使用 HashMap 插入

```

public void createData() {
	Map<String, Object> map = new HashMap<String, Object>();
	map.put("name", "Neo Chen");
	map.put("age", 30);
	map.put("book", new String[]{"Netkiller Linux 手札","Netkiller Java 手札"});
	map.put("website", "http://www.netkiller.cn");
	map.put("email", "netkiller@msn.com");

	IndexResponse response = client.prepareIndex("book", "author", UUID.randomUUID().toString())
			.setSource(map).get();
	System.out.println("Status: " + response.status().getStatus() + "！id=" + response.getId());
}

```

## Get

```

	@Autowired
	private TransportClient client;

	@RequestMapping(value = "/article/{articleId}")
	public GetResponse read(@PathVariable String articleId) {
		GetResponse response = client.prepareGet("information", "article", articleId).get();
		return response;
	}

```

## delete

```

import org.elasticsearch.action.delete.DeleteResponse;  
import org.elasticsearch.client.Client;  

DeleteResponse response = client.prepareDelete("book", "computer", "1")  
.get();  

```

## Search

```

	@RequestMapping(value = "/article/list")
	public List<Map<String, Object>> list() {
		List<Map<String, Object>> list = new ArrayList<Map<String, Object>>();
		SearchResponse response = client.prepareSearch("information").setTypes("article")
				.setSearchType(SearchType.DFS_QUERY_THEN_FETCH).addSort("ctime", SortOrder.DESC)
				.setFrom(0).setSize(60).setExplain(true).get();
		for (final SearchHit hit : response.getHits().getHits()) {
			System.out.println(hit.getSourceAsString());
			hit.getSourceAsMap().remove("content");
			list.add(hit.getSourceAsMap());
		}
		return list;
	}

```

## Query 查询

### match all 匹配所有数据

```

SearchResponse response = client.prepareSearch()
        .setIndices(index)
        .setTypes(type)
        .setSearchType(SearchType.QUERY_AND_FETCH)
        .setFetchSource(new String[]{"title"}, null) 
        .setQuery(QueryBuilders.matchAllQuery())
        .setSize(10).execute().actionGet();		

```

### match 匹配查询

```

public void match() {
	SearchRequestBuilder requestBuilder = client.prepareSearch("company").setTypes("employee")
			.setQuery(QueryBuilders.matchQuery("name", "neo"));
	System.out.println(requestBuilder.toString());

	SearchResponse response = requestBuilder.get();

	System.out.println(response.status());
	if (response.status().getStatus() == 200) {
		for (SearchHit hits : response.getHits().getHits()) {
			System.out.println(hits.getSourceAsString());
		}
	}
}

```

### match phrase 短语精准匹配

```

public void matchPhrase() {
	SearchRequestBuilder requestBuilder = client.prepareSearch("company").setTypes("employee")
			.setQuery(QueryBuilders.matchPhraseQuery("name", "neo"));

	SearchResponse response = requestBuilder.get();

	if (response.status().getStatus() == 200) {
		for (SearchHit hits : response.getHits().getHits()) {
			System.out.println(hits.getSourceAsString());
		}
	}
}

```

## Filter 过滤

```

FilterBuilder filterBuilder = FilterBuilders.andFilter(  
	FilterBuilders.existsFilter("title").filterName("exist"),  
	FilterBuilders.termFilter("title", "elastic")  
);  

SearchResponse response = client.prepareSearch("netkiller")  
	.setPostFilter(filterBuilder)  
	.get();  

```

### term

```

.setPostFilter(QueryBuilders.termQuery("site_id", siteId))

```

### range

```

QueryBuilders.rangeQuery("age").from(12).to(18)

```

## Sorting

```

SearchResponse response = client.prepareSearch("netkiller")  
	.setQuery(QueryBuilders.matchAllQuery())  
	.addSort(SortBuilders.fieldSort("title"))  
	.addSort("_score", SortOrder.DESC)  
	.get() 

```

## 返回 Source 字段

下面例子查询返回 "id", "title", "description", "image", "ctime"

```

	/*
	 * 范例：/restful/search/article/list/23/0/20.json?tags=美国
	 */
	@RequestMapping(value = "/article/list/{siteId}/{from}/{size}")
	public List<Map<String, Object>> listBySiteIdAndTags(@PathVariable String siteId, @PathVariable int from, @PathVariable int size, @RequestParam(value = "tags", required = false) String tags) {
		List<Map<String, Object>> list = new ArrayList<Map<String, Object>>();

		SearchRequestBuilder searchRequestBuilder = client.prepareSearch("information").setTypes("article").setSearchType(SearchType.DFS_QUERY_THEN_FETCH).addSort("ctime", SortOrder.DESC);

		searchRequestBuilder.setFetchSource(new String[] { "id", "title", "description", "image", "ctime" }, null);
		if (tags != null && !tags.equals("")) {
			// logger.info(tags);
			searchRequestBuilder.setQuery(QueryBuilders.matchQuery("tags", tags));
		}
		searchRequestBuilder.setPostFilter(QueryBuilders.termQuery("site_id", siteId)).setFrom(from).setSize(size).setExplain(true);

		logger.info(searchRequestBuilder.toString());
		SearchResponse response = searchRequestBuilder.get();

		for (final SearchHit hit : response.getHits().getHits()) {
			// logger.info(hit.getSourceAsString());
			hit.getSourceAsMap().remove("content");
			list.add(hit.getSourceAsMap());
		}
		logger.info(tags);
		return list;
	}

```

## Count

```

QueryBuilders qb = QueryBuilders.boolQuery().must(QueryBuilders.termQuery("status", false));  

CountResponse response = client.prepareCount("book") // 索引  
                .setQuery(qb) //类型  
                .get(); 

```

## Example 范例

### Spring boot 案例

```

	/*
	 * 范例：/restful/search/article/list/23/0/20.json?tags=美国
	 */
	@RequestMapping(value = "/article/list/{siteId}/{from}/{size}")
	public List<Map<String, Object>> listBySiteIdAndTags(@PathVariable String siteId, @PathVariable int from, @PathVariable int size, @RequestParam(value = "tags", required = false) String tags) {
		List<Map<String, Object>> list = new ArrayList<Map<String, Object>>();

		SearchRequestBuilder searchRequestBuilder = client.prepareSearch("information").setTypes("article").setSearchType(SearchType.DFS_QUERY_THEN_FETCH).addSort("ctime", SortOrder.DESC)
		// .addFields("_source","title", "description", "ctime")
		;

		if (tags != null && !tags.equals("")) {
			// logger.info(tags);
			searchRequestBuilder.setQuery(QueryBuilders.matchQuery("tags", tags));
		}
		searchRequestBuilder.setPostFilter(QueryBuilders.termQuery("site_id", siteId)).setFrom(from).setSize(size).setExplain(true);

		// logger.info(searchRequestBuilder.toString());

		SearchResponse response = searchRequestBuilder.get();

		for (final SearchHit hit : response.getHits().getHits()) {
			// logger.info(hit.getSourceAsString());
			hit.getSourceAsMap().remove("content");
			list.add(hit.getSourceAsMap());
		}
		logger.info(tags);
		return list;
	}
}

```

## FAQ

### 显示查询 JSON 字符串

```
logger.info(searchRequestBuilder.toString());

```

```
2017-09-05 14:34:25 14858 [http-nio-8443-exec-1] INFO  c.e.a.restful.SearchRestController : {
  "from" : 0,
  "size" : 2,
  "query" : {
    "match" : {
      "tags" : {
        "query" : "ioi",
        "operator" : "OR",
        "prefix_length" : 0,
        "max_expansions" : 50,
        "fuzzy_transpositions" : true,
        "lenient" : false,
        "zero_terms_query" : "NONE",
        "boost" : 1.0
      }
    }
  },
  "post_filter" : {
    "term" : {
      "site_id" : {
        "value" : "22",
        "boost" : 1.0
      }
    }
  },
  "explain" : true,
  "sort" : [
    {
      "ctime" : {
        "order" : "desc"
      }
    }
  ]
} 

```