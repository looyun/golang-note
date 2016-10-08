---title: 如何在linux上将GO应用跨平台编译到windows---
起因：我在linux下将一个应用编译为windows应用时调用了sqlite3,出现了以下问题：
>C source files not allowed when not using cgo or SWIG: sqlite3-binding.c

最后从一篇文章上找到了答案并且解决问题，翻译了一下，水平有限，部分无法翻译到位，请见谅。

来源：https://www.limitlessfx.com/cross-compile-golang-app-for-windows-from-linux.html

我最近需要在windows上测试一些应用，虽然GO的跨平台编译直截了当，但应用包含一些C代码并且Arch Linux上mingw64-gcc出现了明显的配置错误。

一番抓狂后我发现了一个临时解决方法直到这个package被修复(task [#4313](https://bugs.archlinux.org/task/42313)),you have to link your Go code against `ssp`.

*note* 虽然这个BUG是Arch Linux特有，但是其他发行版也可能会用同样的方式构建工具链。

# 构建工具链
## 1.下载并解压GO源码*note:*这份指导假定`$GORROT`设定为`/usr/src/go/`

* 用 mercurial (slow) `hg clone -u release https://code.google.com/p/go`* 使用来自于 [golang.org](golang.org)的源码

1a.(推荐，可选) 添加`export PATH="$PATH:$GOROOT/bin"`到你的` ~/.bashrc`(或者你的其他的shell的文件)

## 2.编译你的本地工具链
```cd $GOROOT/src$ ./make.bashInstalled Go for linux/amd64 in /usr/src/go-releaseInstalled commands in /usr/src/go-release/bin```
## 3.不用CGO编译编译你的windows工具链
```cd $GOROOT/src$ env GOOS=windows GOARCH=386 ./make.bash --no-clean.....Installed Go for windows/386 in /usr/src/go-releaseInstalled commands in /usr/src/go-release/bin```如果你想构建64位二进制文件的话用 `GOARCH=amd64`
## 4.用CGO编译windows工具链
* 安装`mingw-w64-gcc`


```sudo pacman -S mingw-w64-gcc.....(1/1) installing mingw-w64-gcc```
```cd $GOROOT/src$ env CGO_ENABLED=1 GOOS=windows GOARCH=386 CC_FOR_TARGET="i686-w64-mingw32-gcc -fno-stack-protector -D_FORTIFY_SOURCE=0 -lssp" ./make.bash --no-clean......Installed Go for windows/386 in /usr/src/go-releaseInstalled commands in /usr/src/go-release/bin```如果你想构建64位二进制文件的话用 `GOARCH=amd64 CC_FOR_TARGET="x86_64-w64-mingw32-gcc ....`*note*：如果BUG被修复，你可以将 `-fno-stack-protector -D_FORTIFY_SOURCE=0 -lssp` 从 `CC_FOR_TARGET`移除.# 实际编译代码使其运行在windows上我们用跟之前一样的环境变量，除了将`CC_FOR_TARGET`换成`CC`## 代码：

```package main

/*static int answerToLife() { return 42;}*/import "C"

import "fmt"

func main() { fmt.Println("the answer to life is", C.answerToLife())}```## 测试：```$ env CGO_ENABLED=1 GOOS=windows GOARCH=386 CC="i686-w64-mingw32-gcc -fno-stack-protector -D_FORTIFY_SOURCE=0 -lssp" go build -o test32.exe$ file test32.exe test32.exe: PE32 executable (console) Intel 80386 (stripped to external PDB), for MS Windows # or build it as a GUI application so Windows won't open up a console window. $ env CGO_ENABLED=1 GOOS=windows GOARCH=386 CC="i686-w64-mingw32-gcc -fno-stack-protector -D_FORTIFY_SOURCE=0 -lssp" go build -ldflags -H=windowsgui -o test32.exe$ file test32.exetest32.exe: PE32 executable (GUI) Intel 80386 (stripped to external PDB), for MS Windows $ wine test32.exethe answer to life is 42```*note:*接下来要用CGO构建本地GO应用的话需要用到`env CC=gcc go build`
