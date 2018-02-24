### 更新apt-get源

- 1 查看当前版本号

```
sudo lsb_release -a
```

获取当前版本的Codename为artful版本

- 2 修改源配置文件

```
vim /etc/apt/sources.list
```

- 3 选择合适的国内镜像 注意保持版本号一致

```
deb http://mirrors.ustc.edu.cn/ubuntu/ artful main restricted universe multiverse
```

- 4 apt-get update


