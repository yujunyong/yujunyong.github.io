title: mysql查询优化
date: 2015-11-05 15:30:55
categories: database
tags: [mysql,optimize]
---
# 查询了不需要的数据
## 查询了一些不需要的行
从文章表里面查询最新的10篇文章，这种查询一般是按时间排序，取最新的10篇文章。如果不加其它的限制则会查询表中所有的记录，这样会导致执行效率降低，增加`limit`可以有效的限制查询的行数，提高效率
## 查询了一些不需要的列
返回电影《Academy Dinosaur》中所有的演员，不要使用下面的查询
``` sql
SELECT * FROM sakila.actor
INNER JOIN sakila.film_actor USING(actor_id)
INNER JOIN sakila.film USING(film_id)
WHERE sakila.film.title = 'Academy Dinosaur';
```
上面的查询会返回3个表中所有的列，但是我们只需要actor表中的字段

应该使用下面的查询
``` sql
SELECT sakila.actor.* FROM sakila.actor
INNER JOIN sakila.film_actor USING(actor_id)
INNER JOIN sakila.film USING(film_id)
WHERE sakila.film.title = 'Academy Dinosaur';
```
