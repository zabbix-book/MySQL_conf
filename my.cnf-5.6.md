[client]
port=3306
socket=/var/lib/mysql/mysql.sock
default-character-set=utf8
#character_set_results=utf8

[mysqld]
port=3306
socket=/var/lib/mysql/mysql.sock
pid_file = /var/lib/mysql/mysql.pid
basedir=/usr
datadir=/var/lib/mysql/data
character-set-server=utf8
default-storage-engine=INNODB
transaction_isolation = READ-COMMITTED
#sql-mode="STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION"
user=mysql
open_files_limit = 65535

##############  Connection ################################
max_connections=5000
connect_timeout=300
interactive_timeout=84600
wait_timeout=84600
max_allowed_packet = 64M
slave_pending_jobs_size_max=64M
thread_cache_size=400
back_log = 1000
max_connect_errors = 1000

#name-resolve
skip-name-resolve          
skip-external-locking
skip-slave-start

##########################################################

#Table
table_open_cache = 2048
table_definition_cache=2048

tmp_table_size = 246M 
max_heap_table_size = 246M

#set timestamp(mysql 5.6)
explicit_defaults_for_timestamp=1
log_bin_trust_function_creators=1

##############  log  #############################
server_id=1
sync_binlog = 1
log_output=file
log_warnings = 2
log_error = /var/lib/mysql/log/error_log/error.log

#复制日志控制
relay_log=/var/lib/mysql/log/relay_log/mysql-relay-bin
relay_log_index=/var/lib/mysql/log/relay_log/mysql-relay-bin.index
#binlog-do-db = ocp
#replicate-ignore-db = information_schema
#log_slave_updates = 1
slave_parallel_workers=4
#read_only=1
##这三个参数是启用binlog/relaylog的校验，防止日志出错
#binlog_checksum = CRC32
#master_verify_checksum = 1
#slave_sql_verify_checksum = 1
##这两个是启用relaylog的自动修复功能，避免由于网络之类的外因造成日志损坏，主从停止
relay_log_purge = 1
relay_log_recovery = 1
##这两个参数会将master.info和relay.info保存在表中，默认是Myisam引擎，官方建议改为InnoDB
#master_info_repository = TABLE
#relay_log_info_repository = TABLE


#添加二进制日志
log_bin= /var/lib/mysql/log/binary_log/mysql-bin
binlog_format = ROW
binlog_row_image = minimal
binlog_cache_size = 8M
expire_logs_days = 10
max_binlog_cache_size = 3G
max_binlog_size = 1G

slow_query_log=1
long_query_time=1
slow_query_log_file=/var/lib/mysql/log/slow_query_log/slow.log
#log_queries_not_using_indexes=1
#log_throttle_queries_not_using_indexes=20
#log-slow-admin-statements=1

#Set General Log
general_log=0
general_log_file = /var/lib/mysql/log/general_log/general.log

#************************ MyISAM Specific options ***********************
myisam_max_sort_file_size=20G
myisam_sort_buffer_size=128M
key_buffer_size=256M 
read_buffer_size=1M 
read_rnd_buffer_size=1M
sort_buffer_size=1M
join_buffer_size =1M
bulk_insert_buffer_size = 64M
myisam_repair_threads = 1
myisam_recover                   
query_cache_type=0
query_cache_size = 0

#************************ INNODB Specific options ************************
innodb_file_per_table=1
#innodb_force_recovery=1
innodb_data_home_dir=/var/lib/mysql/innodb_file/ 
innodb_data_file_path=ibdata1:10M:autoextend
innodb_log_group_home_dir=/var/lib/mysql/innodb_file/
innodb_log_files_in_group=2
innodb_log_file_size=1G

innodb_flush_log_at_trx_commit=2
innodb_open_files=50000
innodb_flush_method=O_DIRECT
innodb_log_buffer_size=16M
innodb_buffer_pool_size=20G   #该参数尽可能的大,物理内存的70-80% . 如16G内存可以设置12G
innodb_additional_mem_pool_size=16M
innodb_thread_concurrency=0
#文件IO的线程数(5.6变更)
innodb_read_io_threads = 16  
innodb_write_io_threads = 8  
innodb_buffer_pool_instances=6
innodb_max_dirty_pages_pct = 75
innodb_lock_wait_timeout = 120
innodb_change_buffering=all
innodb_change_buffer_max_size=25
innodb_purge_threads=1
innodb_purge_batch_size=300
innodb_io_capacity=3000
innodb_doublewrite=ON
innodb_use_native_aio=ON
#innodb_flush_neighbors=ON
#innodb_force_recovery=0

# Dump 缓冲池数据
innodb_buffer_pool_dump_now = ON
innodb_buffer_pool_load_now = ON
innodb_buffer_pool_dump_at_shutdown = ON
innodb_buffer_pool_load_at_startup = ON

[mysqldump]
quick
max_allowed_packet = 64M

[mysql]
disable-auto-rehash
default-character-set = utf8 

[myisamchk]
key_buffer_size = 24M
sort_buffer_size = 36M
read_buffer = 8M
write_buffer = 8M
