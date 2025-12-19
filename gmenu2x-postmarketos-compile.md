# postmarketOS Gmenu2x 编译

本文档介绍如何在 postmarketOS 上编译 Gmenu2x。

## 前置要求

在编译 Gmenu2x 之前，需要确保已安装以下依赖：

1. **SDL 1.2 库**：参考 [SDL 编译文档](https://lualiliu.github.io/website/sdl-opendingux-compile.html)
2. **SDL_ttf 库**：参考 [SDL_ttf 编译文档](https://lualiliu.github.io/website/sdl-ttf-postmarketos-compile.html)
3. **SDL_gfx 库**：参考 [SDL_gfx 编译文档](https://lualiliu.github.io/website/sdl-gfx-postmarketos-compile.html)

## 获取源码

```bash
$ git clone https://github.com/lualiliu/gmenu2x.git
$ cd gmenu2x
```

## 编译依赖

在 postmarketOS 上，使用 `apk` 包管理器安装以下依赖：

```bash
# 基础编译工具
$ sudo apk add build-base make gcc g++

# SDL 1.2 开发库（如果尚未安装）
# 参考 SDL 编译文档
```

## 配置编译选项

Gmenu2x 使用 Cmake 进行编译。

```bash
$ mkdir build
$ cd build
$ cmake ..
$ make
```

通常需要确保以下配置正确：
- `CC`：C 编译器路径
- `CXX`：C++ 编译器路径
- `CFLAGS`：编译选项
- `LDFLAGS`：链接选项
- `SDL_CONFIG`：SDL 配置工具路径（如果使用）

## 编译

```bash
$ make -j$(nproc)
```

如果编译过程中遇到问题，可以尝试：

```bash
# 单线程编译以便查看详细错误信息
$ make

# 清理后重新编译
$ make clean
$ make
```

## 安装

```bash
$ sudo make install
```

如果 Makefile 中没有安装规则，可以手动安装：

```bash
# 安装可执行文件
$ sudo cp gmenu2x /usr/local/bin/

# 安装资源文件（如果有）
$ sudo mkdir -p /usr/local/share/gmenu2x
$ sudo cp -r skins /usr/local/share/gmenu2x/  # 如果存在
```

## 验证

编译完成后，可以通过以下方式验证：

```bash
# 检查编译的可执行文件
$ ls -la gmenu2x

# 检查文件类型
$ file gmenu2x

# 运行程序（如果已安装）
$ gmenu2x
```

## 注意事项

1. **依赖库版本**：确保所有 SDL 相关库都是 SDL 1.2 版本，不能与 SDL 2.0 混用
2. **库路径**：如果依赖库安装到非标准路径（如 `/usr/local`），可能需要设置环境变量：
   ```bash
   $ export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
   $ export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig:$PKG_CONFIG_PATH
   ```
3. **交叉编译**：如果进行交叉编译，需要为目标平台安装相应的开发库
4. **资源文件**：Gmenu2x 可能需要特定的资源文件（如皮肤、字体等），确保这些文件在正确的位置

## 常见问题

### 编译时找不到 SDL 库

确保已安装 SDL 1.2 开发库，并检查能否正确找到 SDL：

```bash
$ pkg-config --modversion sdl
$ sdl-config --version
```

如果返回错误，可能需要：
1. 检查 SDL 是否已正确安装
2. 设置 `PKG_CONFIG_PATH` 环境变量
3. 在 Makefile 中手动指定 SDL 路径

### 编译时找不到 SDL_ttf 或 SDL_gfx

确保已安装 SDL_ttf 和 SDL_gfx 开发库：

```bash
$ pkg-config --modversion SDL_ttf
$ pkg-config --modversion SDL_gfx
```

如果找不到，请参考相应的编译文档进行安装。

### 链接错误

如果遇到链接错误，检查：

1. 所有依赖库是否已正确安装
2. 库文件路径是否正确
3. 是否需要添加额外的链接选项

### 运行时错误

如果程序运行时出错，检查：

1. 所有依赖库是否在系统路径中
2. 资源文件是否在正确位置
3. 运行 `ldd gmenu2x` 检查动态库依赖

[RETURN](https://lualiliu.github.io/website/)

