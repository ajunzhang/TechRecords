#### VSCODE安装GOLANG环境

* 安装VS

* 应用商店安装go插件，目前版本是0.6.61

* 下载依赖的包，且go install。即编译安装这些依赖包，最后在GOPATH/bin目录下应该有对应的exe文件

	go get -u -v github.com/nsf/gocode  
	go get -u -v github.com/rogpeppe/godef  
	go get -u -v github.com/golang/lint/golint  
	go get -u -v github.com/lukehoban/go-outline  
	go get -u -v sourcegraph.com/sqs/goreturns  
	go get -u -v golang.org/x/tools/cmd/gorename  
	go get -u -v github.com/tpng/gopkgs  
	go get -u -v github.com/newhook/go-symbols  
	go get -u -v golang.org/x/tools/cmd/guru  


如下图

![][images/5.24.1.png]


* 为了使用F5调试，我们还需要dlv工具。且go install

	go get -v -u github.com/peterh/liner github.com/derekparker/delve/cmd/dlv  



* 配置setting.json文件

![][images/5.24.2.png]

end

