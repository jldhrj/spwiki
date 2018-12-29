**ss-panel-v3-mod_Uim开发团队仅为面板的用户提供 V2Ray 后端对接功能，V2Ray 节点后端与 ss-panel-v3-mod_Uim 面板无关。ss-panel-v3-mod_Uim 开发团队不对节点后端提供任何形式的保证：不保证后端程序能满足您的要求或您的服务不会中断。如果您因为 V2Ray 节点后端的使用而引发的一切纠纷，应自行准备相关材料，及时与 V2Ray 节点后端开发者联系，ss-panel-v3-mod_Uim 开发团队不承担任何相关责任。**

# V2ray Backend 用法
## 站点设定
首先先在 后台创建节点
> 节点的节点地址为 域名;端口;AlterId;网络层协议(默认 tcp);附加协议;额外参数

事实上在默认情况下 端口为 443 AlterId 为 2. 其中, 在 网络层协议之后的内容可以省略

## 额外参数的格式
额外参数使用 `key=value|another=vale` 这种格式来书写

额外参数中的内容将会 merge 进入导入链接的 json 当中, 也有额外的参数, 通常使用于自动修改后端

| Key | 样例 | 效果 |
| --- | --- | --- |
| dns | 1.1.1.1,8.8.8.8 | 后端的 dns 被设置为 指定值 |


## 使用 v2ray 的伪装方式以及设定方式
| 使用方式协议 | 网络层协议 | 附加协议 | 是否支持自动设定后端 | 设定方式 | 
| --- |-------------| ---| -----| --- |
| TCP | tcp / 无 | 无 | ✅ | N/A |
| KCP | kcp | 无 | ✅ | N/A |
| KCP | kcp | wechat-video 等 | ✅ | N/A |
| TLS | tls | 无 | ❌ | 将自己的证书部署至服务器之后自行修改 config.json |
| Websocket | ws | 无 | ✅ | N/A, 可以在 extraArgs 增加 PATH 设定 |
| TLS + Websocket | tls | ws | ❌ | 参考 tls 和 websocket 的配置方法进行配置 | 


> 单端口多用户请选择 只启用普通端口

并将节点类型选为 `V2Ray`

> 使用 [WHMCS](https://whmcs.indexyz.me/aff.php?aff=1) 购买后端

当你获取了后端之后

## 部署
登陆要部署的服务器 假设这边后端名称为 `bin`

并执行以下命令就可以启动服务器
```bash
chmod +x bin
nano agent.yaml
./bin
```

## Agent 设定
编辑 `agent.yaml` (如果没有就新建一个)
其中的内容为
```yaml
mod:
  nodeId: 节点ID
  key: 通信密钥   # 仅在 web API 接入时启用

database: # 仅在数据库接入时启用
  user: 数据库用户名
  pass: 数据库密码
```

V2Ray 可用的选项为
```yaml 
v2ray:
  alterId: 用户的 alterId
  tag: 传入协议的 Tag (默认为 proxy)
```

## 使用 Docker 运行
您可以使用以下 Dockerfile 来构建您的 Docker Image
```dockerfile
FROM bitnami/minideb
LABEL maintainer="Indexyz <docker@indexyz.me>"

COPY agent /
RUN chmod +x /agent

ENTRYPOINT ["/agent"]
```

将获取到的后端命名为 `agent` 放在这个文件夹, 使用 `docker build -t v2ray-agent .` 来构建您的 Image
然后使用 `docker run -p 443:443 -d --name v2ray v2ray-agent` 来运行

> 为 Docker 运行的方式作了专门的配置项支持, 如果有一个为 mod.nodeId 的配置项, 会自动从 MOD_NODEID 这个环境变量读取值