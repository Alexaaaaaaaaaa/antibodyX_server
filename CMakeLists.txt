cmake_minimum_required(VERSION 3.23)
project(antibodyX_server)


set(CMAKE_CXX_STANDARD 14)
link_directories(/usr/lib64/mysql)
include_directories(include)
include_directories(/usr/include/mysql)

aux_source_directory(src DIR_SRCS)

add_executable(main ${DIR_SRCS})
target_link_libraries(main mysqlclient)
target_link_libraries(main pthread)
