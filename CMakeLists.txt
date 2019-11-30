cmake_minimum_required(VERSION 3.15)
project(cctv_latency_tester CXX)

set(CMAKE_CXX_STANDARD 14)

if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
    message(STATUS "Setting build type to '${default_build_type}' as none was specified.")
    set(CMAKE_BUILD_TYPE "${default_build_type}" CACHE STRING "Choose the type of build." FORCE)
    # Set the possible values of build type for cmake-gui
    set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release")
endif()

set(Sources
        src/App.cpp
        src/App.hpp src/main.cpp src/main.hpp)

find_package( Opencv COMPONENTS core features2d highgui imgproc imgcodecs video REQUIRED )

include_directories( src libs /usr/include /usr/local/include)

if(EXISTS "/opt/local/lib")
    link_directories(/opt/local/lib)
endif()
if(EXISTS "/usr/local/lib")
    link_directories(/usr/local/lib)
endif()

set(CMAKE_CXX_FLAGS         "${CMAKE_CXX_FLAGS} -Wall -fno-strict-aliasing -DLWS_NO_EXTENSIONS")
set(CMAKE_CXX_FLAGS_DEBUG   "-DDEBUG -O0 -g3")
set(CMAKE_CXX_FLAGS_RELEASE "-O3")

add_executable(cctv-latency-tester ${Sources})

set_target_properties(cctv-latency-tester PROPERTIES COMPILE_FLAGS "-include main.hpp")

target_link_libraries(cctv-latency-tester ${OpenCV_LIBS} pthread)

add_custom_target(WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})

set_target_properties(cctv-latency-tester PROPERTIES
        MACOSX_BUNDLE TRUE
        MACOSX_BUNDLE_INFO_PLIST ${CMAKE_CURRENT_SOURCE_DIR}/Info.plist)