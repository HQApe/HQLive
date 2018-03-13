第一步：安装brew
````
$ /usr/bin/ruby -e"$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
````
第二步：安装nginx服务器
````
$ brew tap homebrew/nginx

报错：Error: homebrew/nginx was deprecated. This tap is now empty as all its formulae were migrated.
````
更换一个github的项目(https://github.com/denji/homebrew-nginx)：
````
$ brew tap denji/nginx

$ brew install nginx-full --with-upload-module
````
就能安装成功

安装成功之后可查看信息：
````
$ brew options nginx-full
$ brew info nginx-full
````

第三步：开始启动服务器
````
$ sudo nginx
````
如果发生错误：
````
nginx: [emerg] bind() to 0.0.0.0:1935 failed (48: Address already in use)

nginx: [emerg] bind() to 0.0.0.0:8080 failed (48: Address already in use)

nginx: [emerg] bind() to 0.0.0.0:1935 failed (48: Address already in use)

nginx: [emerg] bind() to 0.0.0.0:8080 failed (48: Address already in use)

nginx: [emerg] bind() to 0.0.0.0:1935 failed (48: Address already in use)

nginx: [emerg] bind() to 0.0.0.0:8080 failed (48: Address already in use)
````
发现前面有尝试过开启，虽然报错了，端口还是被占用了

配置nginx服务
```
// 用vim打开配置文件
vim /usr/local/etc/nginx/nginx.conf
rtmp {
    server {
        listen 1935; // 设置对应的端口
        application mylive {
            live on;
            record off; // 不记录数据
        }
    }
}
```

然后重新load配置文件
````
$ sudo nginx -s reload
````
如果报错:
````
nginx: [error] open() "/usr/local/var/run/nginx.pid" failed (2: No such file or directory)
````
需要重新设置一下配置文件：
````
$ sudo nginx -c /usr/local/etc/nginx/nginx.conf

$ sudo nginx -s reload

$ sudo nginx //开启服务器
````

可能还有一种错误：
````
[emerg] 31225#0: unknown directive "rtmp" in /usr/local/etc/nginx/nginx.conf:9
````
这是因为rtmp 模块没有加进去，需要重新安装一下：
````
$ brew uninstall nginx-full// 删除

$ nginxbrew install nginx-full --with-rtmp-module// 安装
````
利用ffmpeg推送流：
````
$ brew install ffmpeg // 安装ffmpeg
````
