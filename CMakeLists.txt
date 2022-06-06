cmake_minimum_required(VERSION 3.12) # cmake最低版本要求

project(cmake_simple CXX) #声明是c++

##LIB
add_library(cmake_simple_lib STATIC lib.cpp) # STATIC生成静态库lib

#if(BUILD_SHARED_LIBS)
#	target_compile_definitions(cmake_simple_lib PUBLIC SIMPLE_DLL PRIVATE BUILD_DLL)
#   target_compile_options(cmake_simple_lib PRIVATE /W4)
#endif()

#target_include_directories(cmake_simple_lib PUBLIC include)
target_include_directories(cmake_simple_lib PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}> $<INSTALL_INTERFACE:include>)

target_compile_options(cmake_simple_lib PRIVATE /wd4996)
add_executable(cmake_simple_exe main.cpp) #生成exe
#target_include_directories(cmake_simple_exe PUBLIC include)

target_link_libraries(cmake_simple_exe  cmake_simple_lib)

##DLL
add_library(cmake_simple_dll SHARED dll.cpp) #生成动态库dll
#target_include_directories(cmake_simple_dll PUBLIC include) #PUBLIC PRIVATE
target_include_directories(cmake_simple_dll PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}> $<INSTALL_INTERFACE:include>)
#target_link_libraries(cmake_simple_exe cmake_simple_dll)(link error)

install(TARGETS cmake_simple_lib
	EXPORT unofficial-simple-config
	RUNTIME DESTINATION lib
	ARCHIVE DESTINATION lib
)
install(EXPORT unofficial-simple-config DESTINATION share/unofficial-simple)

install (FILES "${CMAKE_CURRENT_SOURCE_DIR}/inclue/lib.h" DESTINATION "include")