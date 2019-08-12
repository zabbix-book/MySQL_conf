# MySQL_conf
本项目是《Zabbix企业级分布式监控系统》第2版书中内容的补充，当前只整理了MySQL5.6版的安装方法，其他版本my.cnf参数略有不同

## 1. 安装MySQL 5.6   
操作系统为CentOS7 X64   

MySQL文件下载地址 http://dev.mysql.com/downloads/repo/yum/    

卸载已存在文件   
```
rpm -qa|grep mariadb|xargs rpm -e --nodeps # 卸载CentOS 7自带的mariadb
rpm -qa|grep -i mariadb #查看是否卸载完毕
wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
```
安装yum仓库文件     
/etc/yum.repos.d/ 目录下新增 mysql-community.repo 、mysql-community-source.repo 
```
yum localinstall mysql-community-release-el7-5.noarch.rpm 
```

安装MySQL软件包    
```
yum install mysql-community-server
```

## 2. 安装完成后无需启动   
确保/var/lib/mysql/目录为空，因为会重新初始化    


## 3. 创建目录    
```
mkdir -p /var/lib/mysql/innodb_file/
mkdir -p /var/lib/mysql/data/
mkdir -p /var/lib/mysql/log/error_log
mkdir -p /var/lib/mysql/log/relay_log
mkdir -p /var/lib/mysql/log/general_log/
mkdir -p /var/lib/mysql/log/binary_log
mkdir -p /var/lib/mysql/log/slow_query_log/
chown -R mysql:mysql /var/lib/mysql
mysql_install_db --datadir=/var/lib/mysql/data/ --user=mysql
```

## 4. 初始化数据库  
```
mysql_install_db --datadir=/var/lib/mysql/data/ --user=mysql
```

## 5.配置mysql参数  

```
wget https://raw.githubusercontent.com/zabbix-book/MySQL_conf/master/my.cnf-5.6.md
```

拷贝my.cnf至server端/etc/my.cnf中  编辑参数/etc/my.cnf中的内存参数 ```innodb_buffer_pool_size=20G ```
```
cp my.cnf-5.6.md /etc/my.cnf
```

## 6. 启动 mysql 服务      
```
systemctl start mysqld.service     #启动 mysql
systemctl enable mysqld.service    #设置 mysql 开机启动
```
