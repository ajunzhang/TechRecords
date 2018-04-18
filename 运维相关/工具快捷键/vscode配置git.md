### vscode配置git

- 下载安装vscode，git.配置git的环境变量。然后在vscode中终端运行如下命令即可：

```
E:\Github\TechRecords>git config --global user.email "zhangsj@hnu.edu.cn"

E:\Github\TechRecords>git config --global user.name "ajunzhang"

E:\Github\TechRecords>git config --global credential.helper store
```

- 从上到下代表的意思为，配置邮箱，用户名。以及记住密码。否则，每次push操作都要输入用户名和密码。