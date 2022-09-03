# SQL优化

# 1、生成和测试环境执行效率不一样:disappointed:

导致因素有很多，比如执行计划、缓存等。但是这次相差了14倍我是没想到的。

## 1.1禁用缓存

使用 `SQL_NO_CACHE`来禁掉缓存,获取真实的速度

`SELECT SQL_NO_CACHE  * from table`  

# 2、索引:facepunch:

## 2.1复合索引

如果建立的是复合索引，索引的顺序要按照建立时的顺序，即从左到右，如: a->b->c（和B﹔树的数据结构有关)
就像是组成一个桥，组成的顺序不对或者缺少就会影响效率。

### 2.1.1不要对索引做如下操作:x:

```
以下用法会导致索引失效
·计算，如:+、-、*、/、!=、<>、is null、 is not null、 or·函数，如: sum()、 round()等等
```

### 2.1.2索引不要放在范围查询右边

```
比如复合索引: a->b->c，当 where a="" and b>10 and 3=""，这时候只能用到a和 b，c用不到索引，因为在范围之后索引都失效（和B+树结构有关)
```

## 2.2减少select *的使用

即:select查询字段和 where中使用的索引字段一致。

## 2.3like模糊搜索

失效情况like " %张三%". like "%张三"
解决方案
使用复合索引，即 like字段是select的查询字段，如: select name from table where name like "%张三%"
`使用 like"张三%"`

如果真要用 select 字段1 from table where 字段2 like %s% 字段1 和字段2 尽量一致
