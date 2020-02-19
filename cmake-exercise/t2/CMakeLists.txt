cmake_minimum_required(VERSION 3.5)
project(HELLO)
# 将src子目录添加到工程，并指定编译输出（包括中间结果和目标二进制）路径为build/bin
# 如果不进行bin目录的指定，那么编译输出（包括中间结果和目标二进制）路径为build/src
add_subdirectory(src bin)	
install(FILES README DESTINATION .)
install(PROGRAMS src/runhello.sh DESTINATION bin)
install(DIRECTORY doc/ DESTINATION doc)

