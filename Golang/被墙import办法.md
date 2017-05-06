#### 远程import支持
GOPATH 指定目录下面有一个src 目录存储go get . 到本地的包
例如从gitHub.como引入```github.com/parnurzeal/gorequest```：
	
	go get github.com/parnurzeal/gorequest


从golang.org引入 ```golang.org/x/net/publicsuffix```：
一般直接执行如下命令无法成功,因为被墙的原因。

	go get golang.org/x/net/publicsuffix
	
可以尝试做如下修改：

	go get github.com/golang/net/publicsuffix

这时下的包在github.com/golang/net目录下，在src目录下手动新建目录golang.org/x/net
然后将net目录下所有文件复制粘贴到golang.org/x/net下面。无需修改go文件中import路径

> 从https://github.com/golang/net下载，然后把目录改成golang.org/x/net。然后，万事大吉。
> ps：有git的话可以直接 go get github.com/golang/net，没有的话自己手动下载放到src目录下即可。