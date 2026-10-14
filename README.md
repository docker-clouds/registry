# docker-clouds/registry

预打包 Docker Registry 镜像制品（docker-save 格式 tgz），供
[kubekey fork](https://github.com/docker-clouds/kubekey)（`volcorch/release-4.0`）在
**artifact export** 阶段下载使用。

## 背景

qingstor 镜像源（`kubekey.pek3b.qingstor.com`）的 `docker-registry-2.8.3-linux-arm64.tgz`
内容实际是 **32 位 ARM**，在 aarch64 节点上会报 `exec format error`。本仓库提供经过
架构验证的正确镜像包。

## 触发方式

```bash
# 方式 1：推 tag（tag 名 = registry 版本，去掉 v 前缀作为产物版本号）
git tag v2.8.3 && git push origin v2.8.3

# 方式 2：手动触发
# Actions → Build Registry Assets → Run workflow → 填 version
```

CI 会：
1. 拉取 `registry:<version>` 的 amd64 + arm64 双架构镜像
2. 分别 `docker save` 为 tgz
3. 创建 GitHub Release 并附带 tgz 产物

## 下载地址（kk fork 10-download.yaml 消费）

```
https://github.com/docker-clouds/registry/releases/download/v<version>/docker-registry-<version>-linux-<arch>.tgz
```

## 产物清单

| 文件 | 说明 |
|---|---|
| `docker-registry-<version>-linux-amd64.tgz` | amd64 Docker Registry 镜像 |
| `docker-registry-<version>-linux-arm64.tgz` | arm64 Docker Registry 镜像 |
