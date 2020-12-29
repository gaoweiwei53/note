# 1. flume架构的组件
**Agent**: 是一个JVM进程，它以**event**的形式将数据从源头送至目的。  
**Event**: flume数据传输的基本单元，由**Header**和**Body**两部分组成，header存放event的一些属性，body存放该条数据。  
# 2. 安装
1) 修改flume-env.sh里的java_home地址
