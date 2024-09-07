## 安装步骤

问下gpt

## 安装的坑

### 【启动异常1】

**表现** 

curl -X GET "localhost:9200/"  

curl: (52) Empty reply from server

```go
Configure other nodes to join this cluster:
• On this node:
  ⁃ Create an enrollment token with bin/elasticsearch-create-enrollment-token -s node.
  ⁃ Uncomment the transport.host setting at the end of config/elasticsearch.yml.
  ⁃ Restart Elasticsearch.
• On other nodes:
  ⁃ Start Elasticsearch with bin/elasticsearch --enrollment-token <token>, using the enrollment token that you generated.
```

**解决**

在 `config/elasticsearch.yml` 中添加或修改以下内容：

```yaml
xpack.security.enabled: false
```

重启 

```bash
./bin/elasticsearch
```



