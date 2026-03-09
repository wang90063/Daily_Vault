---
created: 2026-03-04
status: processed
archived: 2026-03-04
source: user-import
---

1. 设置网路proxy
	- 临时设置（仅在terminal中生效）
```
	export http_proxy=http://127.0.0.1:33210
	export https_proxy=http://127.0.0.1:33210
	export all_proxy=socks5://127.0.0.1:33211
```
	 - 永久设置

2. 遇到了certificate问题，设置如下设置certifacate
```
	/etc/ssl/cert.pem
```