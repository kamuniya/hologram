---
keyword: [Hologres, Hive兼容函数, 解析JSON对象]
---

# GET\_JSON\_OBJECT

GET\_JSON\_OBJECT用于解析JSON对象。本文为您介绍在交互式分析Hologres中GET\_JSON\_OBJECT的用法。

## 使用限制

`GET_JSON_OBJECT`函数位于Hologres的`hive_compatible extension`中，需要实例的Superuser执行`create extension hive_compatible;`创建extension后，才能调用该函数。

## 语法

```
CREATE extension hive_compatible;
SELECT get_json_object ( json_string, path )
```

参数说明如下表所示。

|参数|描述|
|--|--|
|json\_string|JSON对象变量，TEXT类型。格式为合法JSON格式字符串。|
|path|JSON内层对象访问变量。使用`$`表示JSON变量标识，通过`.`或`[]`读取JSON内层对象或数组。如果您输入的JSON字符串无效，则系统返回NULL。 |

## 示例

```
CREATE extension hive_compatible;
Result:
data =
{"store":{
     "fruit":[{"weight":8,"type":"apple"}, {"weight":9,"type":"pear"}],
   "bicycle":{"price":19.95,"color":"red"}},
 "email":"amy@only_for_json_udf_test.net",
 "owner":"amy"
}

SELECT get_json_object(data, '$.owner') FROM test;
Result: amy

SELECT get_json_object(data, '$.name.bicycle.price') FROM test;
Result: 19.95

SELECT get_json_object(data, '$.store.fruit[0]') FROM test;
Result: {"weight":8, "type":"apple"}
```

