---
layout:     post
title:      MyBatisGenerator反引号的使用
subtitle:   文档不细看，百度累死人
date:       2021-11-19
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - MyBatis



---

> 处 富贵 之地， 要 知 贫贱 的 痛痒； 当 少壮 之时， 须 念 衰老 的 辛酸。



一般来说，在 MySql 中，为了防止和官方的保留字起冲突，对于表名、字段名，推荐使用反引号包裹起来，就像这样：  

```` sql
SELECT * FROM `order` LIMIT 1;
````

而在基于 MyBatis中间件的实际开发中，我们常常使用逆向工具 MyBatisGenerator 来生成基本的Mapper文件。  

那么，能否在逆向时 ，就加上反引号呢？  

答案当然是肯定的。  


很多同学都知道，在MyBatisGenerator中的xml配置文件里， `context` 节点下就有关于反引号的配置：

```` xml
<context id="autuanContext"  targetRuntime="MyBatis3">
  		<property name="autoDelimitKeywords" value="true"/>
        <property name="beginningDelimiter" value="`"/>
        <property name="endingDelimiter" value="`"/>
</context>
````

但有些同学根据这个配置信息一配置，发现完全不生效呀，这是怎么一回事呢？ 明明就是按着官网<sup>2</sup>的配置来的呀？  

事实上，上面的配置确实是对的，但是没有配置完全。  

除了需要在  `context` 节点下配置 `property`之外 ，还需要在 `table` 节点下针对表名、字段名再进行单独的配置。  

如下：  

```` xml
<table tableName="order" delimitIdentifiers="true" delimitAllColumns="true" />
````



而关于这些信息，事实上，在官方文档<sup>3</sup>同样有说明：  

> Database Identifiers
> MyBatis Generator (MBG) tries to deal with the case sensitivity of database identifiers automatically. In most cases, MBG is able to find tables regardless of what you specify for catalog, schema, and tableName attributes. MBG's process follows these steps:
>
> If either of the catalog, schema, or tableName attributes contain a space, then MBG will look for tables based on the exact case specified. In this case, MBG will automatically delimit the table identifiers in the generated SQL.
> Else if the database reports that identifiers are stored in upper case, MBG will automatically convert any table identifier to upper case.
> Else if the database reports that identifiers are stored in lower case, MBG will automatically convert any table identifier to lower case.
> Else MBG will look for tables based on the exact case specified.
> In most cases, this process works perfectly. However, there are cases where it will fail. For example, suppose you create a table like this:
>
> create table "myTable" (
>   ...some columns
> )
> Because the table name is delimited, most databases will create the table with the exact case specified - even if the database normally stores identifiers in upper case. In this instance, you should specify the attribute delimitIdentifiers="true" in the table configuration.



另外关于这两个字段也有说明：  

| delimitIdentifiers | Signifies whether MBG should add delimiters to all column names in the generated SQL. This is an alternative to coding a <columnOverride> for every column specifying that the column should be delimited. This is useful for databases like PostgreSQL that are case sensitive with identifiers. The delimiter characters are specified on the <context> element.The default is false. |
| ------------------ | ------------------------------------------------------------ |
| delimitAllColumns  | Signifies whether MBG should add delimiters to all column names in the generated SQL. This is an alternative to coding a <columnOverride> for every column specifying that the column should be delimited. This is useful for databases like PostgreSQL that are case sensitive with identifiers. The delimiter characters are specified on the <context> element. |


所以说，在 context 中配置的 property 确实是正确的，但是如果不配置 table 的话，只有碰到表名中有空格的这种属性，才会启用反引号。  

希望能帮助到各位。  

[1] MyBatisGenerator官网（http://mybatis.org/generator/index.html）
[2] MyBatisGenerator context 节点说明  (http://mybatis.org/generator/configreference/context.html)
[3] MyBatisGenerator table 节点说明  (http://mybatis.org/generator/configreference/table.html)

