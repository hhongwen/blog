---
title: Neo4j使用小结（一）
url: Neo4j-summary-of-usage
date: 2018-12-20 17:00:00
tags: Neo4j
categories: DB
---

## CREATE命令

1. CREATE命令语法：

``` sql
CREATE (
   <node-name>:<label-name>
   {
      <Property1-name>:<Property1-Value>
      ........
      <Propertyn-name>:<Propertyn-Value>
   }
)
```

2. 语法说明：

|语法元素|描述|
|----|----|
|`<node-name>`|它是我们将要创建的节点名称。|
|`<label-name>`|它是一个节点标签名称|
|`<Property1-name>...<Propertyn-name>`|属性是键值对。 定义将分配给创建节点的属性的名称|
|`<Property1-value>...<Propertyn-value>`|属性是键值对。 定义将分配给创建节点的属性的值|

**个人理解：** 要创建一个`<label-name>`的对象名称是`<node-name>`，对象的属性有`<Property1-name>:<Property1-Value>`……`<Propertyn-name>:<Propertyn-Value>`等。

3. 实例：

``` sql
CREATE (hhongwen: Person {name: 'Han Hongwen', born:1992})
```

## MATCH + RETURN命令

> `MATCH`命令不能单独使用，需要与`RETURN`命令结合使用返回结果，或者与更新`SET`命令+`RETURN`命令一起使用。
单独使用会报错：`Neo.ClientError.Statement.SyntaxError: Query cannot conclude with MATCH (must be RETURN or an update clause)……`

1. MATCH命令语法：

``` sql
MATCH (tom {name: "Tom Hanks"}) RETURN tom
/*相当于SQL：*/
select * from <table> where name = 'Tom Hanks'

MATCH (per:Person) RETURN per.name, per.born
/*相当于SQL：*/
select per.name, per.born from Person per

MATCH (nineties:Movie) WHERE nineties.released >= 1990 AND nineties.released < 2000 RETURN nineties.title
/*相当于SQL：*/
select nineties.title from Movie nineties where nineties.released >= 1990 AND nineties.released < 2000
```
