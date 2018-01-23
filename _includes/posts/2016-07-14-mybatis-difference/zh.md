
1. #{}将传入的数据都当成一个字符串，会对自动传入的数据加一个双引号。如：order by id #{sequence}，如果传入的值是desc，那么解析成sql时的值为order by id“desc"。
2. ${}将传入的数据直接显示生成在sql中。如：order by id ${sequence}，如果传入的值是desc，那么解析成sql时的值为order by id desc。
3. #方式能够很大程度防止sql注入。
4. $方式无法防止sql注入。
5. $方式一般用于传入数据库对象，例如传入表名。
6. 一般能用#的就别用$。

