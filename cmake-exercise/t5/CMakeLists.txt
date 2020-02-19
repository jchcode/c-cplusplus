cmake_minimum_required(VERSION 3.5)

# 编译输出库文件
set(libhello_src lib_src/libhello.cpp)
add_library(hello_shared SHARED ${libhello_src})
add_library(hello_static STATIC ${libhello_src})
set_target_properties(hello_shared PROPERTIES SOVERSION 1 VERSION 1.2)
set_target_properties(hello_shared PROPERTIES OUTPUT_NAME "hello_share")

# 安装目标文件，目标文件可能是可执行二进制、共享库、静态库
install(TARGETS hello_shared hello_static
        LIBRARY DESTINATION libshared
        ARCHIVE DESTINATION libstatic)

# 安装普通文件
install(FILES head_src/hello.h DESTINATION include)
install(FILES README DESTINATION .)

# 安装目录，如果目录名不以/结尾，那么这个目录将被安装为目标路径下，
# 如果目录名以/结尾，代表将这个目录中的内容安装到目标路径，但不包括这个目录本身
install(DIRECTORY doc/ DESTINATION doc)