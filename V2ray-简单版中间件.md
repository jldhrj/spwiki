## 项目状态

目前只适配了流量记录、服务器是否在线、在线人数、负载、Speedtest 后端测速、延迟、后端根据前端的设定自动调用 API 增加用户。

v2ray 后端 kcp、tcp、ws 都是多用户共用一个端口。

也可作为 ss 后端一个用户一个端口。

## 项目没有适配的

目前没有限速，和用户ip上报功能，用户ip限制。

## 作为 ss 后端

面板配置是节点类型为 Shadowsocks，普通端口。

加密方式只支持：

- [x] aes-256-cfb
- [x] aes-128-cfb
- [x] chacha20
- [x] chacha20-ietf
- [x] aes-256-gcm
- [x] aes-128-gcm
- [x] chacha20-poly1305 或称 chacha20-ietf-poly1305

## 作为 V2ray 后端

这里面板设置是节点类型v2ray, 普通端口。

支持 kcp、ws、tls 由镜像 Caddy 提供。

[面板设置说明 主要是这个](https://github.com/NimaQu/ss-panel-v3-mod_Uim/wiki/v2ray-%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B)

~~~
没有CDN的域名或者ip;端口（外部链接的);AlterId;协议层;;额外参数(path=/v2ray|host=xxxx.win|inside_port=10550这个端口内部监听))

// ws 示例
xxxxx.com;443;16;ws;;path=/v2ray|host=oxxxx.com|inside_port=10550

// ws + tls (Caddy 提供)
xxxxx.com;443;16;tls;ws;path=/v2ray|host=oxxxx.com|inside_port=10550
~~~

目前的逻辑是

- 如果为外部链接的端口是 443，则默认监听本地127.0.0.1:inside_port，对外暴露443 (如果想用kcp，走443端口，建议设置流量转发)
- 如果外部端口设定不是 443，则监听 0.0.0.0:外部设定端口，此端口为所有用户的单端口，此时 inside_port 弃用。
- 默认使用 Caddy 镜像来提供 tls，控制代码不会生成 tls 相关的配置。

kcp 支持所有 v2ray 的 type：

- none: 默认值，不进行伪装，发送的数据是没有特征的数据包。

~~~
xxxxx.com;xxx换成除了443之外的端口;16;kcp;noop;
~~~

- srtp: 伪装成 SRTP 数据包，会被识别为视频通话数据（如 FaceTime）。

~~~
xxxxx.com;xxx换成除了443之外的端口;16;kcp;srtp;
~~~

- utp: 伪装成 uTP 数据包，会被识别为 BT 下载数据。

~~~
xxxxx.com;xxx换成除了443之外的端口;16;kcp;utp;
~~~

- wechat-video: 伪装成微信视频通话的数据包。

~~~
xxxxx.com;xxx换成除了443之外的端口;16;kcp;wechat-video;
~~~

- dtls: 伪装成 DTLS 1.2 数据包。

~~~
xxxxx.com;xxx换成除了443之外的端口;16;kcp;dtls;
~~~

- wireguard: 伪装成 WireGuard 数据包(并不是真正的 WireGuard 协议) 。

~~~
xxxxx.com;xxx换成除了443之外的端口;16;kcp;wireguard;
~~~

### [推荐] 脚本部署

**脚本说明：**

谷歌云 CentOS、Debian、Ubuntu 适配正常通过。

务必将脚本和生成的 Caddyfile，docker-compose.yml 文件放在同一目录下，并在该目录下运行脚本。

- [x] 脚本适配后端，安装 docker，docker-compose 并启动服务
- [x] 查看日志
- [x] 更新 config
- [x] 更新 images

~~~
mkdir v2ray-agent  &&  cd v2ray-agent
curl https://raw.githubusercontent.com/rico93/shadowsocks-munager/v2ray_api/install.sh -o install.sh && chmod +x install.sh && bash install.sh
~~~