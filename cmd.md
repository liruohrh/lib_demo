# self path

```
lib_demo
--m2
----main.go
--p1
----p1.go
--p2
----p2.go
--main.go
```

# go build
- `go build` 编译包或者文件，必须有仅一个main包的main函数，可以多个声明为main包
- 编译时使用到的官方库(动态库),比如windows/amd64=%GOROOT/pkg/windows_amd64
- 查看的官方源代码=%GOROOT/src
- 查看的官方测试代码=%GOROOT/test
- `go buld -v`
  
  - 编译当前目录,即lib_demo, 有main包以及main函数，因此在当前目录下生成lib_demo.exe
  - 编译main.go时会自动编译其他依赖包
    - ```bash
      lib_demo
      mkdir -p $WORK\b001\
      cat >$WORK\b001\importcfg << 'EOF' # internal
      # import config
      packagefile fmt=A:\compiler\golang\pkg\windows_amd64\fmt.a
      packagefile lib_demo/p2=C:\Users\LYM\AppData\Local\go-build\ed
      		\edd3feccaca0119d615a7a81733f0cb35836f2e797c1cbf734c439047dd33c1c-d
      packagefile runtime=A:\compiler\golang\pkg\windows_amd64\runtime.a
      EOF
      
      cd A:\code\GoJet\mlib\lib_demo
      "%GOROOT%/pkg/tool/windows_amd64/compile.exe" 
      	-o "$WORK/b001/_pkg_.a" -trimpath "$WORK/b001=>" 
      	-p main 
      	-lang=go1.18 
      	-complete 
      	-buildid 1KSBdsEGlEDVk3f2oN5p/1KSBdsEGlEDVk3f2oN5p 
      	-goversion go1.18.1 
      	-c=4 
      	-nolocalimports 
      	-importcfg "$WORK/b001/importcfg" 
      	-pack "A:/code/GoJet/mlib/lib_demo/main.go"
      "%GOROOT%/pkg/tool/windows_amd64/buildid.exe" 
      	-w "$WORK/b001/_pkg_.a" # internal
      	
      cat >$WORK\b001\importcfg.link << 'EOF' # internal
      #.....一系列官方动态库
      packagefile internal/syscall/windows/registry=
      	A:\compiler\golang\pkg\windows_amd64\internal\syscall\windows\registry.a
      	
      modinfo "0w\xaf\f\x92t\b\x02A\xe1\xc1\a\xe6\xd6\x18\xe6path\
      			tlib_demo\nmod\tlib_demo\t(devel)\t\nbuild\t
      -compiler=gc\nbuild\tCGO_ENABLED=1\nbuild\t	
      CGO_CFLAGS=\nbuild\t
      CGO_CPPFLAGS=\nbuild\t
      CGO_CXXFLAGS=\nbuild\t
      CGO_LDFLAGS=\nbuild\t
      GOARCH=amd64\nbuild\t
      GOOS=windows\nbuild\t
      GOAMD64=v1\n\xf92C1\x86\x18 r\x00\x82B\x10A\x16\xd8\xf2"
      EOF
      
      
      
      mkdir -p $WORK\b001\exe\
      cd .
      "%GOROOT%/pkg/tool/windows_amd64/link.exe" 
      	-o "$WORK/b001/exe/a.out.exe" 
      	-importcfg "$WORK/b001/importcfg.link" 
      	-buildmode=pie 
      	-buildid=XR5GTX4omFYtDgsofnnr/1KSBdsEGlEDVk3f2oN5p
      	/1KSBdsEGlEDVk3f2oN5p/XR5GTX4omFYtDgsofnnr 
      	-extld=gcc "$WORK/b001/_pkg_.a"
      "%GOROOT%/pkg/tool/windows_amd64/buildid.exe" 
      	-w "$WORK/b001/exe/a.out.exe" # internal
      	
      mv $WORK\b001\exe\a.out.exe lib_demo.exe
      ```
    - Go 编译器使用 $WORK 目录作为编译过程中的工作目录，并使用 $WORK\b001\exe\a.out.exe 作为生成的可执行文件路径。
    
      - 具体而言，它执行了以下步骤：使用 $WORK\b001\importcfg.link 中指定的依赖关系和符号信息，将当前代码包的源代码编译为目标平台的机器码。
      - 使用 $WORK\b001\exe\a.out.exe 作为生成的可执行文件路径，在其中将所有目标文件链接成一个单独的可执行文件。
      - 使用 buildid.exe 工具为该可执行文件添加构建 ID（在这里是 qYR6LhOcCf2lPylwF1bf/1KSBdsEGlEDVk3f2oN5p/Tco7RGOcU-ZCrfd02PLr/qYR6LhOcCf2lPylwF1bf）和调试信息。
      - 重命名目标文件为 lib_demo.exe。需要注意的是，此示例中使用了 -buildmode=pie 和 -extld=gcc 选项，它们分别表示使用位置无关代码和 GCC 进行链接，
        以生成更安全、更可靠的可执行文件。
- `go buld -v lib_demo/m2`
  - 在当前目录下生成m2.exe