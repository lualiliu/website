# postmarketOS SDL_ttf 编译

本文档介绍如何在 postmarketOS 上编译 SDL_ttf 的 SDL-1.2 分支。

## 前置要求

在编译 SDL_ttf 之前，需要确保已安装 SDL 1.2 库。如果尚未安装，请参考 [SDL 编译文档](https://lualiliu.github.io/website/sdl-opendingux-compile.html)。

## 获取源码

```bash
$ git clone -b SDL-1.2 https://github.com/libsdl-org/SDL_ttf.git
$ cd SDL_ttf
```

## 编译依赖

在 postmarketOS 上，使用 `apk` 包管理器安装以下依赖：

```bash
# 基础编译工具
$ sudo apk add build-base autoconf automake libtool

# FreeType 开发库（SDL_ttf 需要）
$ sudo apk add freetype-dev

# SDL 1.2 开发库（如果尚未安装）
$ sudo apk add sdl-dev
```

## 配置编译选项

如果源码目录中没有 `configure` 脚本，需要先运行 `autogen.sh`：

```bash
$ ./autogen.sh
```

然后运行 `configure` 进行配置：

```bash
$ ./configure --prefix=/usr/local
```

如果需要指定 SDL 的安装路径，可以使用：

```bash
$ ./configure \
    --prefix=/usr/local \
    --with-sdl-prefix=/usr/local
```

## 编译

```bash
$ make -j$(nproc)
```

## 安装

```bash
$ sudo make install
```

安装完成后，可能需要更新动态链接库缓存：

```bash
$ sudo ldconfig
```

## 验证

编译完成后，可以通过以下方式验证：

```bash
# 检查编译的库文件
$ ls -la .libs/libSDL_ttf*.so*

# 检查安装的库文件
$ ls -la /usr/local/lib/libSDL_ttf*

# 检查头文件
$ ls -la /usr/local/include/SDL/SDL_ttf.h
```

## 注意事项

1. **SDL 1.2 依赖**：SDL_ttf 的 SDL-1.2 分支必须与 SDL 1.2 版本配合使用，不能与 SDL 2.0 混用
2. **FreeType 版本**：确保安装的 FreeType 开发库版本与运行时库版本兼容
3. **交叉编译**：如果进行交叉编译，需要为目标平台安装相应的开发库
4. **库路径**：如果安装到非标准路径（如 `/usr/local`），可能需要设置 `LD_LIBRARY_PATH` 环境变量

## 常见问题

### 编译时找不到 SDL 库

确保已安装 SDL 1.2 开发库，并检查 `configure` 脚本能否正确找到 SDL：

```bash
$ pkg-config --modversion sdl
```

如果返回错误，可能需要手动指定 SDL 路径或安装 SDL 开发包。

### 编译时找不到 FreeType 库

确保已安装 `freetype-dev` 包：

```bash
$ sudo apk add freetype-dev
```

### 运行时找不到库文件

如果程序运行时提示找不到 `libSDL_ttf.so`，可以：

1. 检查库文件是否已安装到系统路径
2. 设置 `LD_LIBRARY_PATH` 环境变量：
   ```bash
   $ export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
   ```
3. 运行 `ldconfig` 更新动态链接库缓存

### configure 脚本不存在

如果源码目录中没有 `configure` 脚本，需要先运行：

```bash
$ ./autogen.sh
```

如果 `autogen.sh` 也不存在，可能需要安装 `autotools` 相关包：

```bash
$ sudo apk add autoconf automake libtool
```

[RETURN](https://lualiliu.github.io/website/)

