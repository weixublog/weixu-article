#MySQL中的 IF 条件语句的用法

> `IF( expr1 , expr2 , expr3 )`

expr1 的值为 TRUE，则返回值为 expr2
expr1 的值为FALSE，则返回值为 expr3

###IF 表达式

> `IF( expr1 , expr2 , expr3 )`

expr1 的值为 TRUE，则返回值为 expr2
expr1 的值为FALSE，则返回值为 expr3

如下：

```
SELECT IF(TRUE,1+1,1+2);
-> 2

SELECT IF(FALSE,1+1,1+2);
-> 3

SELECT IF(STRCMP("111","222"),"不相等","相等");
-> 不相等
```

那么这个 IF 有啥用处呢？举个例子：
查找出售价为 50 的书，如果是 java 书的话，就要标注为 已售完
那么对应的SQL语句该怎样去写呢？

```
select *,if(book_name='java','已卖完','有货') as product_status from book where price =50
```

###IFNULL 表达式

> `IFNULL( expr1 , expr2 )`

在 expr1 的值不为 `NULL`的情况下都返回 expr1，否则返回 expr2，如下：

```
SELECT IFNULL(NULL,"11");
-> 11

SELECT IFNULL("00","11");
-> 00
```

