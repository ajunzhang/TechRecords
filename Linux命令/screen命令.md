####  Screen
GNU Screen可以看作是窗口管理器的命令行界面版本。它提供了统一的管理多个会话的界面和相应的功能。

###  screen [-AmRvx -ls -wipe][-d <作业名称>][-h <行数>][-r <作业名称>][-s ][-S <作业名称>]

    参数说明

    -A 　将所有的视窗都调整为目前终端机的大小。
    -d <作业名称> 　将指定的screen作业离线。
    -h <行数> 　指定视窗的缓冲区行数。
    -m 　即使目前已在作业中的screen作业，仍强制建立新的screen作业。
    -r <作业名称> 　恢复离线的screen作业。
    -R 　先试图恢复离线的作业。若找不到离线的作业，即建立新的screen作业。
    -s 　指定建立新视窗时，所要执行的shell。
    -S <作业名称> 　指定screen作业的名称。
    -v 　显示版本信息。
    -x 　恢复之前离线的screen作业。
    -ls或--list 　显示目前所有的screen作业。
    -wipe 　检查目前所有的screen作业，并删除已经无法使用的screen作业。

======================================================================================================
### 常用screen命令

    screen -dmS lookup // 创建一个名字为 lookup的screen
    screen -r // 查看全部screen
    screen -r lookup //进入lookup这个screen
    screen -d lookup //离线lookup这个screen

    screen -r lookup //进入lookup这个screen
    exit //删除该screen

    screen -r lookup //进入lookup这个screen
    Ctrl+c // 停止启动的lookup这个服务