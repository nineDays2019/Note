# cmake [01]

From 《CMake Practice》

## 变量

* `PROJECT_BINARY_DIR`	编译发生的当前目录
* `EXECUTABLE_OUTPUT_PATH`	二进制的输出路径
* `LIBRARY_OUTPUT_PATH`	库的输出路径
* `CMAKE_INSTALL_PREFIX`	类似于 configure 脚本的 -prefix，常见的使用方法是这样的：`cmake -CMAKE_INSTALL_PREFIX=/usr .`

## 指令

* `INSTALL`	用于定义安装规则，安装的内容包括目标二进制、动态库、静态库以及文件、目录、脚本等等
* `INCLUDE_DIRECTORIES`		用来向工程添加多个特定的头文件搜索路径，路径之间用空格分割，如果路径中包含了空格，可以使用双引号将它括起来，默认的行为是追加到当前的头文件搜索路径的后面
* `TARGET_LINK_LIBRARIES`	可以用来为 target 添加需要链接的共享库
* 特殊的环境变量 `CMAKE_INCLUDE_PATH` 和 `CMAKE_LIBRARY_PATH` 	这两个是**环境变量**而不是 cmake 变量，使用方法是要在 bash 中用 export 进行设置

## Notice

* 在哪里 `ADD_EXECUTABLE` 或 `ADD_LIBRARY`，如果需要改变目标存放路径，就在哪里加入 `EXECUTABLE_OUTPUT_PATH` 或 `LIBRARY_OUTPUT_PATH` 的定义。
* `CMAKE_INSTALL_PREFIX` 默认值是 **/usr/local**
* 静态库要使用 `ARCHIVE` 关键字
* `make VERBOSE=1` 可以查看错误细节
* `otool -L` [mac]	`ldd`	[linux]