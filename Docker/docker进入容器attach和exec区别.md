### Docker attach

Docker attach可以attach到一个已经运行的容器的stdin，然后进行命令执行的动作。 但是需要注意的是，如果从这个stdin中exit，会导致容器的停止。

### docker exec

使用-it时，则和我们平常操作console界面类似。而且也不会像attach方式因为退出，导致整个容器退出。 ctrl+D退出