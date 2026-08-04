# VSCode-Codex-Connection-Issues

> [English](README.md) | [中文](README.zh-CN.md)

> 解决 VS Code Codex 登录失败、一直 Reconnecting、卡在 Thinking 等常见问题；提供完整 SSH 配置、代理转发、远程开发环境搭建方案，适合在 AutoDL / 极算云 / 腾讯云等 GPU 服务器上使用 AI 编程插件。

![license](https://img.shields.io/badge/License-MIT-green) ![platform](https://img.shields.io/badge/Platform-Windows%2FmacOS%2FLinux-lightgrey)

## 背景

在 VS Code 里通过 SSH 连接远程 GPU 服务器使用 Codex 插件时，插件经常连不上 Codex 服务：一直显示 "Reconnecting…"、登录页转圈、对话卡在 "Thinking"。本地连接通常正常，问题出在远程环境访问不到你本地的代理 / VPN。

本教程给出完整修复方案：通过 SSH 把你的本地 VPN 代理端口转发到远程服务器，再配置远程 VS Code 的代理设置，最后验证连通性。

## 常见症状

- Codex 一直显示 Reconnecting…；
- 登录失败或一直转圈；
- 卡在 Thinking 没有响应；
- 本地正常，只在 VS Code Remote（SSH）里失败。

![重连问题](docs/images/reconnecting-issue.png)

![登录问题](docs/images/login-issue.png)

## 快速修复（5 步）

### 第 1 步：查看 VPN 代理端口

先看魔法梯子的 VPN 代理端口是多少，我们以 10090 为例，有的客户端是 7890。

![VPN 代理端口](docs/images/vpn-proxy-port.png)

### 第 2 步：在 SSH 配置里转发端口

打开本机 `C:\Users\（你的用户名）\.ssh\config`，用记事本等编辑器打开，在自己服务器字段下面加入：

```
RemoteForward 10090 127.0.0.1:10090
```

然后保存。

![主机配置](docs/images/ssh-config.png)

### 第 3 步：打开远程设置（JSON）

连接服务器，打开 VSCode，按 `Ctrl+Shift+P`，在顶端输入 `Preferences: Open Remote Settings (JSON)`，选图里的这个，一般都是第一个。

![打开远程设置](docs/images/open-remote-settings.png)

### 第 4 步：添加代理设置

打开后添加这两行（注意有逗号），记得保存：

```json
"http.proxy": "http://127.0.0.1:10090",
"https.proxy": "http://127.0.0.1:10090",
```

![远程设置](docs/images/remote-settings-json.png)

### 第 5 步：重启并验证

保存完重启 VSCode，最后最好在服务器终端输入：

```bash
netstat -tuln | grep 10090
```

测试是否可以联通，出现端口监听就 OK 了。

![测试端口](docs/images/test-port.png)

## 注意事项

- 不管是 10090 还是 7890，都以梯子的实际端口号为准，SSH 配置和代理设置里的端口都要一致；因为一些梯子由于版本原因只能使用 7890。
- 确保本地能登录 Codex，先排除梯子节点问题。
- 以上操作不行，就重启 VSCode 或者梯子。

## 常见问题

**Q1：端口到底用多少？**
以你的 VPN 客户端实际监听的端口为准，本教程只是以 10090 举例，很多客户端是 7890。

**Q2：服务器上需要改系统配置吗？**
不需要。`RemoteForward` 会把本地端口自动转发到服务器，服务器侧无需额外系统配置。

**Q3：全部操作后还是 Reconnecting？**
重启 VSCode 和 VPN；如果服务器所在网络受限，尝试更换梯子节点。

## 目录结构

```
VSCode-Codex-Connection-Issues/
├── README.md                    # 英文说明
├── README.zh-CN.md              # 中文说明
├── LICENSE / .gitignore
└── docs/
    └── images/                  # 教程截图
        ├── login-issue.png
        ├── reconnecting-issue.png
        ├── vpn-proxy-port.png
        ├── ssh-config.png
        ├── open-remote-settings.png
        ├── remote-settings-json.png
        └── test-port.png
```

## 许可证

MIT
