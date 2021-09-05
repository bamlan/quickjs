#zqjsc

clmakez.sh使用clang编译zqjsc;

zqjsc添加-z输出模式：
    -za 输出 main 函数到 (-o 选项文件名).c文件，输出 js 的 bytecode 到 (-o 选项文件名).h文件;
    -zh 仅输出 js 的 bytecode 到 (-o 选项文件名).h文件;
    -zc 仅输出 main 函数到 (-o 选项文件名).c文件

zqjsc 独立输出 .c 和 .h文件可以让我们修改程序的c代与js代码互不影响。
使用示例：
如果我们要开发一个运行在linux服务模式下的quickjs应用，可以使用以下步骤：
    1：编写好quickjs下的js逻辑代码（js模块、js运行入口单元）, 或者写一个仅包含如 print("helloword"); 语句的简单 js 运行入口文件；
    2：使用 “zqjsc -za -o 程序名 js运行入口.js”, 会在当前目录下生成 “程序名.c” 和 “程序名.h” 两个文件；
    3：修改 “程序名.c” 文件，在 main 函数中加入 daemon 相关的代码；
    4：现在可以再次开发服务程序的js逻辑代码，完成后使用 “zqjsc -zh -o 程序名 js运行入口.js” 生成 “程序名.js”，js改动的模块、单元代码的bytecode会在输出到 .h文件中，
       .c 文件不用修改，.c文件编译时会自动加载修改后的js相关模块与单元代码；
    5：使用 gcc 或 clang 编译输出的 .c 文件，即可生成更新js功能后的服务程序；
    6：修改 js 部分，重复 4、5 步骤即可。
