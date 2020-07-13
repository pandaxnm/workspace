# 自用workspace
## CentOS 安装 Docker
命令如下:
```
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```
或者使用 daocloud:
```
curl -sSL https://get.daocloud.io/docker | sh
```

安装完成，启动Docker
```
sudo systemctl start docker
```

## 安装 Docker Compose
命令如下：
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
赋予可执行权限
```
sudo chmod +x /usr/local/bin/docker-compose
```
创建软链接
```
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```
测试是否安装成功
```
docker-compose --version
```

## 使用
克隆
```
git clone https://github.com/pandaxnm/workspace.git
```
转到目录
```
cd workspace/files
```
build & up
```
docker-compose up
```

### Composer
用 docker-compose 进行操作
```
docker-compose run --rm -w /data/www/appname php-fpm composer update
```
或者进入容器
```
cd workspace/app
docker run -it --rm -v `pwd`:/data/www/ -w /data/www/appname files_php-fpm composer update
```
## 目录结构
- `app/` - 网站应用
- `file/nginx/conf.d/` nginx配置文件目录
- `file/.env` 环境变量

## Docker Hub镜像更改
创建或修改 /etc/docker/daemon.json：
```
{
    "registry-mirrors": [
        "https://docker.mirrors.ustc.edu.cn",
        "https://registry.docker-cn.com"
    ]
}
```
然后
```
sudo systemctl daemon-reload
sudo systemctl restart docker
```




