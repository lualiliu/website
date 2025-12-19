# OpenDingux SDL 编译

本文档介绍如何编译 OpenDingux 的 SDL 1.2 分支，支持 kmsdrm 和 fbcon 视频驱动。

## 获取源码

```bash
$ git clone https://github.com/OpenDingux/SDL.git
$ cd SDL
$ git checkout opendingux
```

## 编译依赖

确保已安装以下依赖：

```bash
# 基础编译工具
$ sudo apt-get install build-essential autoconf automake libtool

# DRM/KMS 开发库（用于 kmsdrm 支持）
$ sudo apt-get install libdrm-dev

# 假如是postmarketOS
$ sudo apk add g++ gcc autogen linux-headers autoconf make 
```

## 配置编译选项

### 支持 kmsdrm 和 fbcon

```bash
$ ./autogen.sh
$ ./configure \
    --enable-video-kmsdrm \
    --enable-video-fbcon \
    --disable-video-x11 \
    --disable-video-directfb \
    --prefix=/usr/local
```

## 编译

```bash
$ make -j$(nproc)
```

## 安装

```bash
$ sudo make install
```

## 验证

编译完成后，可以通过以下方式验证：

```bash
# 检查编译的库文件
$ ls -la .libs/libSDL*.so*

# 运行测试程序（如果可用）
$ cd test
$ make
```

## 注意事项

1. **kmsdrm 驱动**：需要内核支持 DRM/KMS，通常需要 Linux 3.12+ 内核
2. **fbcon 驱动**：需要内核支持 framebuffer 设备（/dev/fb0）
3. **运行时选择驱动**：可以通过环境变量 `SDL_VIDEODRIVER=kmsdrm` 或 `SDL_VIDEODRIVER=fbcon` 来选择使用的视频驱动
4. **交叉编译**：根据目标设备调整交叉编译工具链路径和架构

## 常见问题

### 编译时找不到 DRM 库

确保已安装 `libdrm-dev` 包，如果使用交叉编译，需要目标平台的 libdrm 开发库。

### 运行时无法初始化视频

检查内核是否支持相应的驱动，并确保有相应的设备节点（如 `/dev/dri/card0` 用于 kmsdrm）。

[RETURN](https://lualiliu.github.io/website/)

