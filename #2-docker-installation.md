# Docker Installation

## Installation on Servers that have access to Internet
**Commands:**
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

sudo apt-get update

sudo apt-get install docker-ce=17.03.2~ce-0~ubuntu-xenial -y
```

## Installation on Servers that **do not** have access to Internet

1. Download debian package "Docker 17.03.2" on any linux server that have access to Internet  
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
sudo apt-get update
sudo apt-get download docker-ce=17.03.2~ce-0~ubuntu-xenial

```
2. Copy the debian package (downloaded in step 1) to server where it needs to be installed. Once copied, run the following commands
```
sudo dpkg -i <path-of-the-debian-package-copied>
#Verify if the docker is installed
docker version
```
>Client:
>Version:      17.03.2-ce
>API version:  1.27
>Go version:   go1.7.5
>Git commit:   f5ec1e2
>Built:        Tue Jun 27 03:35:14 2017
>OS/Arch:      linux/amd64

>Server:
>Version:      17.03.2-ce
>API version:  1.27 (minimum version 1.12)
>Go version:   go1.7.5
>Git commit:   f5ec1e2
>Built:        Tue Jun 27 03:35:14 2017
>OS/Arch:      linux/amd64
>Experimental: false

