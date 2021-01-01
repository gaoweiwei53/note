# 1. 什么是Hbase
# 2. 数据模型
- Namespace: 相当于数据库的概念，里面包含一组表。
- Column Family:
- Row
- Column
- Version: 时间戳
# 3. 安装配置
问题：启动之后Hmaster不久就消失了  
解决：在hbase-site.xml中添加
