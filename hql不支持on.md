---
title: hql不支持on
date: 2018-05-15 09:25:10
tags: hql
---
hql语句报错

    from BreakCase b left join b.item t on b.item=t.id
> org.hibernate.hql.ast.QuerySyntaxException: unexpected token: on near line 1,

原因是因为HQL不支持ON这个字符，如果要做关联关系就必须将On改为where

    from BreakCase b left join b.item t where b.item=t.id

> 注意：此处breakCase与item是一对多的关系