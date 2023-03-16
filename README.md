# antibodyX_server used for:
Use CMakelists.txt for configuration. You can use CLion to remotely connect to the compiled webserver
# 说明：
目的：可使用本地Clion远程连接Linux服务器进行调试编译
# 注意事项：
1、CMakelists.txt中的最下面两行【动态/静态链接库链接只能指明在add_executable的后面】

target_link_libraries(files mysqlclient)

target_link_libraries(files pthread)

2、表示显式动态链接库链接【即使Linux上面的LD_LIBRARY_PATH/LIBRARY_PATH/（/etc/ld.so.conf文件）均设置了动态链接库的位置，CMakelist.txt文件仍然需要指明（表示build时需要进行目标链接，否则无法找到函数）】

3、因为mysqlclient的实际库函数并不在头文件【mysql/mysql.h】中，需要去安装的mysql-devel开发包里面主动找到动态链接库


# 注意坑
如果出现错误：terminate called after throwing an instance of 'std::regex_error'
what():  regex_error
表明你的GNU编译器版本低于4.9.0或vs2013以下，请先升级编译器，因为regex在低版本中的确实现了头文件regex.h，但是gcc库文件并没有跟上，导致实例化出错！
