---
type: concept
created: 2026-03-04
area: "[[SE_CodeBase]]"
tags: [environment, configuration]
---

# 网络与SSL配置

在搭建 Gemini CLI 或 OrbitOS 环境时，确保网络畅通和证书有效是基础。主要涉及 **终端代理设置** 和 **系统 SSL 证书路径** 的指定。

## 核心配置
- **代理**: 需覆盖 `http`, `https` 和 `all_proxy` (socks5)。
- **证书**: 指定系统全局证书路径 `/etc/ssl/cert.pem`。

## 相关概念
- [[网络与SSL配置指南]]
- [[项目结构与原理深度探索]]
