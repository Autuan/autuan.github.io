---
layout:     post
title:      使用ChatGPT将MyBatis代码格式化
subtitle:   chatgpt promote
date:       2023-06-09
author:     Autuan.Yu
header-img: https://cdn.pixabay.com/photo/2017/10/23/20/45/memory-2882481_640.jpg
catalog: true
tags:
    - MyBatis
    - ChatGPT
---

最近写了一个ChatGPT 的 promote, 可以用来帮助我们优化 Mybatis 中的 SQL 代码，现在发出来和各位探讨，如果有遗漏或更好的promote，欢迎大家留言指正~


# promote

---
优化下面 MyBatis中的xml文件代码,数据库是MySQL，注意格式及缩进，所有的MYSQL 关键词要使用大写，所有关于数据库、表名、字段使用反引号括起，不要省略任何别名，以markdown代码格式xml风格响应,每一行最长不超过60个字符：

``` xml

${要格式化的 MyBatis 代码}

```

---

# 测试

## input

``` xml
<select id="promoteTestAutuan" resultMap="baseMap">
select m.id,m.name,m.avatar,m.mobile
       from member m
       <where>
       <if test="id != null">
and m.id = #{id}
        </if>
     <if test="mobile != null and mobile != ''">
        and m.mobile like #{mobile}
     </if>
   </where>
</select>
```

## output

``` xml
<select id="promoteTestAutuan" resultMap="baseMap">
  SELECT
    m.id,
    m.name,
    m.avatar,
    m.mobile
  FROM
    `member` m
  <where>
    <if test="id != null">
      AND m.id = #{id}
    </if>
    <if test="mobile != null and mobile != ''">
      AND m.mobile LIKE CONCAT('%',#{mobile},'%')
    </if>
  </where>
</select>

```

可读性是不是提升了许多呢 :)
