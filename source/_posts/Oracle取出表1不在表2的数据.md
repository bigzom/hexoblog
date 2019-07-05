---
title: Oracle高效取出表1不在表2的数据
date: 2019-07-05 16:09:55
tags: [Oracle]
---
##  注意表B中不要有null
```sql
select * from T_A  a
left join T_B b on a.id=b.id
where b.id is null;
```