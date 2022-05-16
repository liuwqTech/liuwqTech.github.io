---
title: SpringBoot整合ES
tags: SpringBoot
categories:
- 后端
- SpringBoot
- 整合
cover: https://tva1.sinaimg.cn/large/e6c9d24ely1h0m874mzigj21kb0u0tdx.jpg

---

# SpringBoot整合ES

## ES简介

​       Elasticsearch是一个基于[Lucene](https://baike.baidu.com/item/Lucene/6753302)的搜索服务器。它提供了一个分布式多用户能力的[全文搜索引擎](https://baike.baidu.com/item/全文搜索引擎/7847410)。

​       官网查看对应的版本： https://docs.spring.io/spring-data/elasticsearch/docs/4.2.1/reference/html/##new-features

## 安装ES

### 安装ES

```bash
##下载es的镜像
docker pull elasticsearch:7.16.3
##创建es的容器 并启动  single-node单机
docker run -d --name es -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.16.3
##测试
http://localhost:9200/
```

- 验证：浏览器进入：http://localhost:9200/，看到如下界面即为安装运行成功。

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h0m6uvsdy3j20za0g0ac6.jpg" style="zoom:33%;" />

### 安装kibana

​       为了界面显示的更为友好，或者说为了我们能够看到一些界面，我们再安装一下kibana。

```bash
##下载kibana的镜像
docker pull kibana:7.16.3
##准备kibana的配置文件
docker inspect es的容器id（docker ps -a 查看）
## 找到  "IPAddress": "172.17.0.2" 找出es对应的容器ip地址
## Default Kibana configuration for docker target
server.name: kibana
server.host: "0"
elasticsearch.hosts: [ "http://172.17.0.2:9200" ]
xpack.monitoring.ui.container.elasticsearch.enabled: true

##启动kibana
docker run -d --restart=always --log-driver json-file --log-opt max-size=100m --log-opt max-file=2 --name kibana -p 5601:5601 -v /Users/liuwq/docker/es/kibana.yml:/usr/share/kibana/config/kibana.yml kibana:7.16.3
##测试
http://localhost:5601/
```

- 验证：浏览器进入：http://localhost:5601/，看到如下界面即为安装运行成功。

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h0m874mzigj21kb0u0tdx.jpg" style="zoom: 33%;" />

### 简单使用

- 获取所有数据

```bash
##命令：获取所有数据
GET /_search
```

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h0m8bqdabkj212g0u043x.jpg" style="zoom:33%;" />

- 添加数据

  ```javascript
   PUT /ecommerce/product/1
    {
        "name" : "Apple ipad pro 1T",
        "desc" :  "苹果 平板电脑",
        "price" :  11999,
        "producer" : "Apple",
        "tags": [ "Apple", "ipad" ]
    }
  ```

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h0m8fj0rw2j216y0miado.jpg" style="zoom:33%;" />

- 查询数据

```javascript
GET /ecommerce/product/1
```

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h0m8gj93bbj216q0qgjvb.jpg" style="zoom:33%;" />

- 修改数据

```javascript
POST /ecommerce/product/1/_update
{
    "doc" : {
        "name" : "Apple ipad mini 64g"
    }
}
```

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h0m8hxevotj216s0lagp0.jpg" style="zoom:33%;" />

- 删除数据

```javascript
DELETE /ecommerce/product/1
```

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h0m8ipdtxzj216m0l6adc.jpg" style="zoom:33%;" />

**注：删除完可以再查看一下，发现查询不到数据了。**

## SpringBoot 整合

### 添加依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
</dependency>
```

### 常用注解

- Document

```java
//标示映射到Elasticsearch文档上的领域对象
public @interface Document {
  //索引库名次，mysql中数据库的概念
    String indexName();
  //文档类型，mysql中表的概念
    String type() default "";
  //默认分片数
    short shards() default 5;
  //默认副本数量
    short replicas() default 1;
}
```

- Id

```java
//表示是文档的id，文档可以认为是mysql中主键的概念
  public @interface Id {
  }
```

- Field

```java
public @interface Field {
    //文档中字段的类型
      FieldType type() default FieldType.Auto;
    //是否建立倒排索引
      boolean index() default true;
    //是否进行存储
      boolean store() default false;
    //分词器名次
      String analyzer() default "";
  }
```

- FieldType

```java
//为文档自动指定元数据类型
  public enum FieldType {
      Text,//会进行分词并建了索引的字符类型
      Integer,
      Long,
      Date,
      Float,
      Double,
      Boolean,
      Object,
      Auto,//自动判断字段类型
      Nested,//嵌套对象类型
      Ip,
      Attachment,
      Keyword//不会进行分词建立索引的类型
  }
```

### 写配置

```properties
##es的配置
spring.elasticsearch.rest.uris=http://192.168.200.100:9200
spring.data.elasticsearch.repositories.enabled=true
spring.data.elasticsearch.client.reactive.endpoints=192.168.200.100:9200
```

### 代码

- model.es—Article

```java
package com.example.es.model.es;

import lombok.Data;
import org.springframework.data.annotation.Id;
import org.springframework.data.elasticsearch.annotations.Document;
import org.springframework.data.elasticsearch.annotations.Field;
import org.springframework.data.elasticsearch.annotations.FieldType;

import java.util.List;

//注意indexName要小写
@Document(indexName = "blog")
@Data
public class Article {
    @Id
    private String id;
    private String title;
    @Field(type = FieldType.Nested, includeInParent = true)
    private List<Author> authors;

    public Article(String title) {
        this.title = title;
    }
}
```

- model.es—Author

```java
package com.example.es.model.es;

import lombok.Data;

@Data
public class Author {
    private String name;
    public Author(String name) {
        this.name = name;
    }
}
```

- domain.repository

```java
package com.example.es.domain.repository;

import com.example.es.model.es.Article;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.elasticsearch.repository.ElasticsearchRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface ArticleRepository extends ElasticsearchRepository<Article,String> {

    //根据作者名称搜索
    Page<Article> findByAuthorsName(String name, Pageable pageable);

    //搜索title字段
    Page<Article> findByTitleIsContaining(String word,Pageable pageable);

    //根据标题搜索
    Page<Article> findByTitle(String title,Pageable pageable);
}
```

- 增删改查测试

```java
package com.example.es;

import com.alibaba.fastjson.JSON;
import com.example.es.domain.repository.ArticleRepository;
import com.example.es.model.es.Article;
import com.example.es.model.es.Author;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.elasticsearch.core.ElasticsearchRestTemplate;
import org.springframework.data.elasticsearch.core.SearchHits;
import org.springframework.data.elasticsearch.core.mapping.IndexCoordinates;
import org.springframework.data.elasticsearch.core.query.NativeSearchQueryBuilder;
import org.springframework.data.elasticsearch.core.query.Query;

import java.util.Arrays;

import static java.util.Arrays.asList;
import static org.elasticsearch.index.query.QueryBuilders.regexpQuery;

@SpringBootTest
class EsApplicationTests {

    @Autowired
    private ArticleRepository articleRepository;

    //增
    @Test
    void testSave() {
        Article article = new Article("Spring Data Elasticsearch");
        article.setAuthors(asList(new Author("god"),new Author("John")));
        articleRepository.save(article);

        article = new Article("Spring Data Elasticsearch2");
        article.setAuthors(asList(new Author("god"),new Author("King")));
        articleRepository.save(article);

        article = new Article("Spring Data Elasticsearch3");
        article.setAuthors(asList(new Author("god"),new Author("Bill")));
        articleRepository.save(article);
    }

    //查
    @Test
    public void queryAuthorName()  {
        Page<Article> articles = articleRepository.findByAuthorsName("chali", PageRequest.of(0,10));
        for (Article article : articles.getContent()) {
            System.out.println(article);
            for (Author author : article.getAuthors()) {
                System.out.println(author);
            }
        }
    }

    //改
    @Test
    public void update() {
        Page<Article> articles = articleRepository.findByTitle("Spring Data Elasticsearch",PageRequest.of(0,10));

        Article article = articles.getContent().get(0);
        System.out.println(article);
        System.out.println(article.getAuthors().get(0));
        Author author = new Author("chali");
        article.setAuthors(Arrays.asList(author));
        articleRepository.save(article);
    }

    //删
    @Test
    public void delete(){
        Page<Article> articles = articleRepository.findByTitle("Spring Data Elasticsearch",PageRequest.of(0,10));
        Article article = articles.getContent().get(0);
        articleRepository.delete(article);
    }

    //老版本Template查询
    @Autowired
    private ElasticsearchRestTemplate elasticsearchRestTemplate;
    //使用Template进行关键字查询
    //关于正则表达式可以参考https://www.runoob.com/java/java-regular-expressions.html
    //.*data.* 可以匹配ddata, dataa等
    @Test
    void queryTileContainByTemplate()  {
        Query query = new NativeSearchQueryBuilder().withFilter(regexpQuery("title",".*elasticsearch2.*")).build();
        SearchHits<Article> articles = elasticsearchRestTemplate.search(query, Article.class, IndexCoordinates.of("blog"));
        System.out.println(JSON.toJSONString(articles));
    }

}
```

## ElasticsearchRepository

| 关键字              | 使用示例                           | 等同于的ES查询                                               |
| :------------------ | :--------------------------------- | :----------------------------------------------------------- |
| And                 | findByNameAndPrice                 | {“bool” : {“must” : [ {“field” : {“name” : “?”}}, {“field” : {“price” : “?”}} ]}} |
| Or                  | findByNameOrPrice                  | {“bool” : {“should” : [ {“field” : {“name” : “?”}}, {“field” : {“price” : “?”}} ]}} |
| Is                  | findByName                         | {“bool” : {“must” : {“field” : {“name” : “?”}}}}             |
| Not                 | findByNameNot                      | {“bool” : {“must_not” : {“field” : {“name” : “?”}}}}         |
| Between             | findByPriceBetween                 | {“bool” : {“must” : {“range” : {“price” : {“from” : ?,”to” : ?,”include_lower” : true,”include_upper” : true}}}}} |
| LessThanEqual       | findByPriceLessThan                | {“bool” : {“must” : {“range” : {“price” : {“from” : null,”to” : ?,”include_lower” : true,”include_upper” : true}}}}} |
| GreaterThanEqual    | findByPriceGreaterThan             | {“bool” : {“must” : {“range” : {“price” : {“from” : ?,”to” : null,”include_lower” : true,”include_upper” : true}}}}} |
| Before              | findByPriceBefore                  | {“bool” : {“must” : {“range” : {“price” : {“from” : null,”to” : ?,”include_lower” : true,”include_upper” : true}}}}} |
| After               | findByPriceAfter                   | {“bool” : {“must” : {“range” : {“price” : {“from” : ?,”to” : null,”include_lower” : true,”include_upper” : true}}}}} |
| Like                | findByNameLike                     | {“bool” : {“must” : {“field” : {“name” : {“query” : “? *”,”analyze_wildcard” : true}}}}} |
| StartingWith        | findByNameStartingWith             | {“bool” : {“must” : {“field” : {“name” : {“query” : “? *”,”analyze_wildcard” : true}}}}} |
| EndingWith          | findByNameEndingWith               | {“bool” : {“must” : {“field” : {“name” : {“query” : “*?”,”analyze_wildcard” : true}}}}} |
| Contains/Containing | findByNameContaining               | {“bool” : {“must” : {“field” : {“name” : {“query” : “?”,”analyze_wildcard” : true}}}}} |
| In                  | findByNameIn(Collectionnames)      | {“bool” : {“must” : {“bool” : {“should” : [ {“field” : {“name” : “?”}}, {“field” : {“name” : “?”}} ]}}}} |
| NotIn               | findByNameNotIn(Collectionnames)   | {“bool” : {“must_not” : {“bool” : {“should” : {“field” : {“name” : “?”}}}}}} |
| True                | findByAvailableTrue                | {“bool” : {“must” : {“field” : {“available” : true}}}}       |
| False               | findByAvailableFalse               | {“bool” : {“must” : {“field” : {“available” : false}}}}      |
| OrderBy             | findByAvailableTrueOrderByNameDesc | {“sort” : [{ “name” : {“order” : “desc”} }],”bool” : {“must” : {“field” : {“available” : true}}}} |