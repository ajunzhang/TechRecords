
Ubuntu17.10安装docker步骤如下：

- sudo apt-get update
- sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
- curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
- sudo apt-key fingerprint 0EBFCD88
 
- sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu zesty stable"
 
- sudo apt-get update
- sudo apt-get install docker-ce

- service docker start