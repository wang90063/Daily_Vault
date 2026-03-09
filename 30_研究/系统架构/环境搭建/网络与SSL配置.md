---
type: reference
created: 2026-03-04
area: "[[SE_CodeBase]]"
tags: [environment, networking, ssl, configuration]
status: complete
---

# 网络与SSL配置指南

## 概述
本笔记记录了在安装和配置 Claude Code CLI 及 OrbitOS 过程中遇到的网络代理和 SSL 证书问题的解决方法。

## 1. 网络代理设置 (Proxy Setup)
为了在受限网络环境下正常访问 API，需要在终端中配置代理。

### 临时设置（仅在当前 Terminal 会话生效）
```bash
export http_proxy=http://127.0.0.1:33210
export https_proxy=http://127.0.0.1:33210
export all_proxy=socks5://127.0.0.1:33211
```

### 永久设置
将上述命令添加到您的 shell 配置文件中（如 `~/.bashrc` 或 `~/.zshrc`）。

## 2. SSL 证书配置 (Certificate Issues)
如果遇到 SSL 证书验证失败的问题，可以尝试指定系统的证书文件路径。

### 证书路径
在 macOS 或某些 Linux 发行版中，证书通常位于：
```text
export NODE_EXTRA_CA_CERTS=/etc/ssl/cert.pem
```

## 3. 配置 Claude API Key

```bash
export ANTHROPIC_BASE_URL="https://www.packyapi.com"
export ANTHROPIC_AUTH_TOKEN="sk-CEwlzutxzUBjoBLwmrtVzP140bxFhnwMBxY2rFXWiHvckEhw"
```
---
**相关阅读:**
- [[OrbitOS]]
- [[项目结构与原理深度探索]]
