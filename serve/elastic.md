### Elasticsearch

`elasticsearch.yml`

```yml
xpack.security.enabled: true # 开启密码
xpack.license.self_generated.type: basic
xpack.security.transport.ssl.enabled: true

# 允许外网访问
network.host: 0.0.0.0
http.cors.enabled: true
http.cors.allow-origin: "*"
cluster.initial_master_nodes: ["node-1", "node-2"]
```

进入bin目录 设置密码 cmd输入

```
elasticsearchbat
```

设置密码 新建一个cmd 窗口

```bash
elasticsearch-setup-passwords interactive
```

进入bin目录 注册成服务  cmd输入

```bash
elasticsearch-service.bat install
```





**analysis-ik 分词器**

[下载对应版本的ik分词器](https://github.com/medcl/elasticsearch-analysis-ik)

1. 解压到 `elasticsearch-7.xx.xx/plugins`下

2. 将解压后的文件夹：`plugins/elasticsearch-analysis-ik-7.17.16` ，重新命名为：`plugins/analysis-ik` 

3. 将`es目录/plugins/analysis-ik/config目录` 重新命名为 `analysis-ik`，并剪切到 `es目录/config/` 

4. 在`es目录/config/analysis-ik` 目录下编辑`IKAnalyzer.cfg.xml`文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
   <properties>
   	<comment>IK Analyzer 扩展配置</comment>
   	<!--用户可以在这里配置自己的扩展字典 -->
   	<entry key="ext_dict">my_ext.dic</entry>
   	 <!--用户可以在这里配置自己的扩展停止词字典-->
   	<!--<entry key="ext_stopwords"></entry>-->
   	<!--用户可以在这里配置远程扩展字典 -->
   	<!-- <entry key="remote_ext_dict">words_location</entry> -->
   	<!--用户可以在这里配置远程扩展停止词字典-->
   	<!-- <entry key="remote_ext_stopwords">words_location</entry> -->
   </properties>
   ```

5. 在`es目录/config/analysis-ik` 目录下新增一个my_exit.dit文件，文件每行的词为自定义分词，如：

   ```dic
   坤哥
   奥里给
   阿西吧吧
   ```

6. 重启 es 。（如果有kibana也要重启）

7. 测试 在kibana 或 其他工具测试 
   ```http
   GET /_analyze
   {
     "analyzer": "ik_max_word",
     "text": "厉不厉害你坤哥。"
   }
   
   # 由于添加了 "坤哥" 自定义分词 会分词出 "token" : "坤哥"
   # ik_smart：最少切分
   # ik_max_word：最细切分
   ```

   



### Kibana

`kibana.yml`

```yml
# 允许外网访问
server.host: "0.0.0.0"

# es账号密码 注意：是在es设置的kibana用户名密码 默认是kibana_system
elasticsearch.username: "kibana_system"
elasticsearch.password: "密码"

# 设置web页面为中文
i18n.locale: "zh-CN"
```













