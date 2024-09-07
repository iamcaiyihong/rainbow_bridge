## 官网
https://www.elastic.co/guide/en/elasticsearch/reference/current/run-elasticsearch-locally.html

## gpt
问gpt: 怎么使用docker安装es和kinbana，不使用密码？
```yaml
version: '3.8'

services:
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:8.6.2
    container_name: elasticsearch
    environment:
      - discovery.type=single-node
      - xpack.security.enabled=false
    ports:
      - "9200:9200"
    networks:
      - es-network

  kibana:
    image: docker.elastic.co/kibana/kibana:8.6.2
    container_name: kibana
    environment:
      - ELASTICSEARCH_HOSTS=http://elasticsearch:9200
    ports:
      - "5601:5601"
    depends_on:
      - elasticsearch
    networks:
      - es-network

networks:
  es-network:
    driver: bridge

```

执行 ``docker-compose up``  即可

## 使用kinbana，访问es
常见的命令有: 创建index、创建doc、搜索doc、更新doc、删除doc
使用kinbana进行模糊搜索

## 教程
https://www.elastic.co/search-labs/tutorials/search-tutorial/full-text-search