
# QuickJS Javascript Engine 

Authors: Fabrice Bellard and Charlie Gordon

Ported from https://bellard.org/quickjs/ and its official GitHub mirror https://github.com/bellard/quickjs

# zqjsc编译器

从 QuickJS 的编译器 `qjsc` 修改而来，增加了 `-z[a|h|c]` 输出模式

* 编译zqjsc：
    参考 `clmakez.sh` 脚本使用 `clang` 编译 `zqjsc`;

* `-z` 输出模式：
    * `-za` 输出 main 函数到 (-o 选项文件名).c 文件，输出 js 的 bytecode 到 (-o 选项文件名).h 文件;
    * `-zh` 仅输出 js 的 bytecode 到 (-o 选项文件名).h 文件;
    * `-zc` 仅输出 main 函数到 (-o 选项文件名).c 文件

zqjsc 独立输出 .c 和 .h文件可以让我们修改程序的c代与js代码互不影响。

# zqjsc 使用

如果我们要开发一个运行在 linux 服务模式下的 quickjs 应用，可以参考以下步骤：

* 示例步骤
    1. 编写好 quickjs 环境下执行的 js 逻辑代码（js模块、js运行入口单元）, 或者写一个仅包含如 `print("helloword");` 的简单的 js 文件；
 
    2. 运行 `zqjsc -za -o 程序名 js运行入口.js` , 会在当前目录下生成 `程序名.c` 和 `程序名.h` 两个文件；
    
    3. 修改 `程序名.c` 文件，在 main 函数中加入 daemon 相关的代码；
    
    4. 现在可以再次开发服务程序的 js 逻辑代码，完成后运行 `zqjsc -zh -o 程序名 js运行入口.js` 生成 `程序名.h`，js改动的模块、单元代码的 bytecode 会输出到这个文件中，`.c` 文件不用修改，编译后程序会自动加载修改后的 js 相关模块与单元代码的 bytecode；
    
    5. 使用 gcc 或 clang 编译输出的 .c 文件，例如：`clang 程序名.c -o 程序名 -flto -I /path/to/quickjs/include -L /path/to/quickjs/lib /path/to/libquickjs.lto.a -lm -ldl -lpthread`，即可生成更新 js 功能后的服务程序；
    
    6. 再次修改 js 部分，重复 4、5 步骤即可。
