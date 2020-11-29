# idea
1. dependencies are not found 或者 fail to **
解决：
- 删除仓库里的update文件重新试试
- 项目的SDK可能没有指定！！
2. .gitignore文件好像可以放在任意位置
/ 表示忽略文件夹

3. Startup Error: Unable to detect graphics environment
```bash
sudo vim ~/.bashrc 
export DISPLAY=:0.0
ource ~/.bashrc 
```
# Spark
