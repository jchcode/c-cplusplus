include_directories("/tmp/cmake/t3/include")
link_directories("/tmp/cmake/t3/libshared" "/tmp/cmake/t3/libstatic")
add_executable(main main.cpp)
target_link_libraries(main hello libhello.a)