需要安装SDL1.2库和scons库

```

$ cd
$ wget https://github.com/steward-fu/rg99/releases/download/v1.1/gambatte.tar.gz
$ tar xvf gambatte.tar.gz
$ cd gambatte
$ ./build_sdl.sh

```

如编译失败,在./gambatte_sdl/SConstruct中增加conf.CheckLib('SDL_image')

```
/bin/ld: menu.o: in function `init_menu()':
menu.cpp:(.text+0x4d3): undefined reference to `IMG_LoadPNG_RW'
/bin/ld: menu.cpp:(.text+0x4ff): undefined reference to `IMG_LoadPNG_RW'
collect2: error: ld returned 1 exit status
scons: *** [gambatte_sdl] Error 1
```
