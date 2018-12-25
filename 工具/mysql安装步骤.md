## mysql免安装版配置步骤

## 此应该是5.7之前的版本
1. 配置环境变量
2. 以管理员身份运行CMD，进入mysql的bin目录，输入 mysqld --install
3. 输入mysqld  --initialize
4. 此时进入mysql应该是进不去的（没有显示临时密码啊摔），直接重置root密码， 输入net stop mysql 关闭mysql服务
5. 输入mysqld --skip-grant-tables 
6. 另开一个新的CMD窗口，输入mysql 登入，use mysql;
7. 输入 update user set authentication_string=password('123456') where user='root'; 修改密码
8. 修改提示成功后，关闭mysqld.exe进程，关闭CMD，再重启mysql服务，进入后再设置一次密码即可

## 5.7版本