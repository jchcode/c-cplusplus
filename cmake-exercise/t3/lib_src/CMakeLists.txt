set(libhello_src hello.cpp)
add_library(hello SHARED ${libhello_src})
add_library(hello_static STATIC ${libhello_src})
set_target_properties(hello PROPERTIES SOVERSION 1 VERSION 1.2)
set_target_properties(hello_static PROPERTIES OUTPUT_NAME "hello")
install(TARGETS hello hello_static
        LIBRARY DESTINATION libshared
        ARCHIVE DESTINATION libstatic)
install(FILES hello.h DESTINATION include)
