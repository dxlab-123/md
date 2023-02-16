# Elasticsearch

## 设置elasticsearch一次最大数量查询

```
put   ip/_settings?preserve_existing=true

{
  "index.max_result_window": 2147483647
}
```

![image-20220322111131160](https://raw.githubusercontent.com/dxlab-123/typore/main/img/202209141601249.png)

## 提高elasticsearch中index（索引）的fields（字段）的上限

***方法一：未成功***

```
put   ip/_settings?preserve_existing=true

{
  "index.mapping.total_fields.limit": 5000
}
```

![image-20220322130321672](https://raw.githubusercontent.com/dxlab-123/typore/main/img/202209141601653.png)

***方法二：（iot平台）***

将存储方式改为json存储，即物模型的存储方式为json-string。