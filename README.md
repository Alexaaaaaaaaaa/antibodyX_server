# antibodyX_server
# introduction:
Use CMakelists instead of makelist for configuration. You can use CLion to remotely connect to the linux server and build your project.
# 简介：
目的：可使用本地Clion远程连接Linux服务器进行调试编译
# 注意事项：
1、CMakelists.txt中的最下面两行【动态/静态链接库链接只能指明在add_executable的后面】

target_link_libraries(files mysqlclient)

target_link_libraries(files pthread)

2、表示显式动态链接库链接【即使Linux上面的LD_LIBRARY_PATH/LIBRARY_PATH/（/etc/ld.so.conf文件）均设置了动态链接库的位置，CMakelist.txt文件仍然需要指明（表示build时需要进行目标链接，否则无法找到函数）】

3、因为mysqlclient的实际库函数并不在头文件【mysql/mysql.h】中，需要去安装的mysql-devel开发包里面主动找到动态链接库


# 注意坑
如果出现错误：terminate called after throwing an instance of 'std::regex_error' what():  regex_error

表明你的GNU编译器版本低于4.9.0或vs2013以下，请先升级编译器，因为regex在低版本中的确实现了头文件regex.h，但是gcc库文件并没有跟上，导致实例化出错！


# Clion远程连接Linux调试项目遇到的问题如下：
## centOS7安装mysql8：

1、rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022  
Mysql的最新安装密钥GBG【用于解决报错：GPG key retrieval failed: [Errno 14] curl#37 - "Couldn’t open file /etc/pki/rpm-gpg/RPM-GPG-KEY-EPE】

2、sudo rpm -Uvh http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
解决yum找不到mysql-server包的问题【安装这个包后，会获得两个mysql的yum repo源：/etc/yum.repos.d/mysql-community.repo，/etc/yum.repos.d/mysql-community-source.repo】

3、sudo yum install mysql-server

4、systemctl start mysqld

5、新安装的mysql默认无密码或存在随机密码，用以下方式设置root密码：
无密码登录方式：
/data/software/mysql8/bin/mysql -u root --skip-password
有密码登录方式（初始的随机密码在/data/mysql8_data/mysql/mysql.log下）
mysql -u root -p
password:随机密码
修改密码：
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';
刷新权限：
FLUSH PRIVILEGES;


## 'mysql/mysql.h' file not found的头文件错误
sudo yum install mysql-devel

安装后使用命令locate mysql.h找到mysql.h位置，通常在/usr/include/mysql目录下，在CMakeLists.txt添加：include_directories(/usr/include/mysql)以解决问题