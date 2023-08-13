####  基于nextcloud/all-in-one 26.04版本做的国内适配
## 解决以下问题：
1. nextcloud-aio基础组件，`nextcloud-aio-apache`, `nextcloud-aio-nextcloud`, `nextcloud-aio-postgresql`, `nextcloud-aio-redis`, `nextcloud-aio-notify-push`更换为国内阿里云Docker镜像
2. 对于需要HTTPS的用户，利用Caddy可以自动化获取和管理SSL证书的特性，增加了一层caddy反向代理。
3. Caddy默认SSL证书获取需要`80`和`443`端口可以公网访问，但在国内私有化布署，布署在家庭宽带网络下，由于无法开放`80`和`443`端口，需要使用`AliDNS`插件，用阿里云DNS的API进行域名认证。本项目包含了集成`AliDNS`插件的定制化`caddy`镜像。
4. 原镜像中`alpine`启动安装`apk`网络太慢，本镜像定制化修改了`nextcloud/aio-nextcloud`镜像，更换为`ustc`源
5. 原镜像默认SSL端口为443，国内网络需要更改为非443端口，在配置文件中增加了NC_PORT选项，支持非443标准端口访问。

## 安装方法
1. 将`sample.conf`复制为`.env`，修改其中配置项。其中：

- `DATABASE_PASSWORD` 为数据库默认密码
- `NEXTCLOUD_PASSWORD` 为nextcloud中admin帐户的初始密码
- `REDIS_PASSWORD` 为Redis服务默认密码
相关的密码建议随机密码生成器（https://suijimimashengcheng.bmcx.com/)生成复杂密码以确保安全。


- `NC_HOST` 为域名（不包含端口）
- `NC_PORT` 为HTTPS访问端口

- `NEXTCLOUD_DATADIR` 为本地存储路径

- `ALIYUN_ACCESS_KEY_ID` 和 `ALIYUN_ACCESS_KEY_SECRET` 为阿里云DNS API访问密钥

2. 将`.env`文件修改完成，即可使用命令：
```bash
docker compose up -d
```
启动镜像集

使用命令：
```bash
docker compose logs -f
```
查看启动日志