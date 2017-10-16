

最近在引入了一个 framework 后，在 Generate dSYM 的时候报出了大量的 warning： warning: \(armv7\) /Users/scmbuild/workspace/standard-pay/IOS/cp\_record\_client\_release\_729146/2134/mspc\_iphone\_msdk/AlipaySDK4Standard/AlipaySDK/Library/UTDID.framework/UTDID\(UTDIDAES.o\) unable to open object file: No such file or directory

在 Xcode 查看编译过程发现是执行 dsymutil 这条命令时产生的。 dsymutil links the DWARF debug information found in the object files for an executable exe\_path by using debug symbols information contained in its symbol table.

众所周知，Xcode 编译的时候会处理两种符号：

Mach-O 符号：链接器在链接的时候需要处理。 调试符号：顾名思义，在使用调试器调试用到的符号。 为了让发行的安装包更小，通常会在编译的时候将调试符号从可执行文件中去掉。这样，在发生 crash 后得到的 crash log 里面只能得到 16 进制的地址。

Xcode 提供了一个编译设置项：Debug Information Format，有两个选项：DWARF 和 DWARF with dSYM File。

其中，DWARF 是一种独立于语言和操作系统的调试文件格式。最初是设计用来配合 ELF（ Executable and Linkable Format，精灵和矮人 :\]）工作的。DWARF 不会在可执行文件中包含调试符号，而是仅仅包含对 .o 文件的引用，而这些 .o 文件才真正包含调试符号。

dSYM 是 Xcode 用来存储调试符号的文件，可以用它来符号化 crash log 或者调试程序。

如果 Debug Information Format 设置成了 DWARF with dSYM File，Xcode 在编译结束时会调用 dsymutil，将 .o 中的符号抽出来生成 .dSYM 文件。

（生成 .dSYM 文件需要消耗一定的时间，因为 debug 的时候本地有 .o 文件，可以不需要 .dSYM 文件来 debug，所以最好将 debug 版本的 Debug Information Format 设置成 DWARF。）

回头来看最初的问题。使用

1 nm -a /path/to/foo.framework/foo 来查看所有的符号，其中就包含了

1 000000005704a7a2 - 03 0001 OSO /Users/USERNAME/Library/Developer/Xcode/DerivedData/VIVerifyCore-gfjrgtjfcqfqpybbcyouhfavymdn/Build/Intermediates/FRAMEWORKNAME.build/Debug-iphonesimulator/FRAMEWORKNAME.build/Objects-normal/armv7/SOMECLASS.o 由此可见，因为引入的 framework 编译时开启了 Generate Debug Symbols，编译出来的二进制文件包含了对一些 .o 文件的引用。这些 .o 文件是 framework 在编译的过程中产生的，framework 的使用方在执行 dsymutil 时找不到它们，于是产生 warning。

在关闭 Generate Debug Symbols 后重新编译 framework，执行同样的命令，生成的可执行文件中就没有这些引用了。再集成到 App 中，编译就不会有前面提到的 warning。

小结 : Xcode「Build Settings」→「Build Options」→「Debug Information Format」→ DWARF。设置即可

[http://blog.cocoachina.com/article/45884](http://blog.cocoachina.com/article/45884)

