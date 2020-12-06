# Git
1. git pull origin master  
refusing to merge unrelated histories:
s: Git pull origin master –allow-unrelated-histories
2. Key is invalid. You must supply a key in OpenSSH public key format  
s: cat命令将 ~/.ssh/id_rsa.pub 内容输出到终端，再拷贝
3. git配置远程地址  
`git remote add origin `

4. git删除远程地址  
`git remote rm origin`

5. 查看远程仓库地址信息  
`git remote -v `

```
echo "# latex" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M master
git remote add origin https://github.com/gaoweiwei53/latex.git
git push -u origin master
```

# IDEA
- Ctrl+/: for single line comments (//...)  
Ctrl+Shift+/: for block comments (/*...*/)、

- Ctrl + P: 查看方法参数
