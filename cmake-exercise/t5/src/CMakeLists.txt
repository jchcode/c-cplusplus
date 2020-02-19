cmake_minimum_required(VERSION 3.5)

# 对安装的头文件与库文件进行测试
project(HELLO)
include_directories(/tmp/cmake/t5/include)
link_directories(/tmp/cmake/t5/libshared /tmp/cmake/t5/libstatic)
add_executable(hello main.cpp)
target_link_libraries(hello hello_share hello_static)

# 安装目标文件，目标文件可能是可执行二进制、共享库、静态库
install(TARGETS hello
        RUNTIME DESTINATION bin)
# 安装非目标文件的可执行程序安装，比如脚本之类的
install(PROGRAMS runhello.sh DESTINATION bin)