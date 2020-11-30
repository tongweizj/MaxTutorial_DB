# v5.8
https://www.jianshu.com/p/3821c2603b92

在mysql中执行下面语句修改密码

```
show databases；
use mysql;
update user set authentication_string=PASSWORD("yourpassword") where user='root';
update user set plugin="mysql_native_password";
flush privileges;
quit;
```
