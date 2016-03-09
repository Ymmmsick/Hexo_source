title: 'mysql单表多timestamp报错#1293 - Incorrect table definition; there can be only one TIMESTAMP column with C'
date: 2016-02-24 16:45:57
categories: MySql
tags: mysql,1293
---

今天在用SqlYog导入sql备份文件的时候又遇到了错误，而且不是往常修改配置文件的错误，点击打开错误文件，alway crashed！！！猜测应该是微软记事本太渣的原因，于是转到sqlyog安装目录，用submit打开错误文件，拉到最下面找到错误，根据提示定位到sql文件中的行数，原来是mysql单表多timestamp报错。

一个表中出现多个timestamp并设置其中一个为current_timestamp的时候经常会遇到

> _#1293 - Incorrect table definition; there can be only oneTIMESTAMP column with CURRENT_TIMESTAMP in DEFAULT or ON UPDATEclause


<font color=red size=3>原因是当你给一个timestamp设置为on updatecurrent_timestamp的时候，其他的timestamp字段需要显式设定default值,但是如果你有两个timestamp字段，但是只把第一个设定为current_timestamp而第二个没有设定默认值，mysql也能成功建表,但是反过来就不行...</font>


红色的解释是网上的，实际上<font color=red size=3>on updatecurrent_timestamp（在navicat中文版中为“刷新当前时间戳计时”选项）</font>只能设置一个，或者不设置都可以，不能同时设置2个及以上。