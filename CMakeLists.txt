cmake_minimum_required(VERSION 3.0)
project(stockfish C CXX)
if("${CMAKE_BUILD_TYPE}" STREQUAL "")
  message(STATUS "Setting Build type==> Release")
  set(CMAKE_BUILD_TYPE "Release")
endif()

set(CMAKE_SKIP_BUILD_RPATH FALSE)
set(CMAKE_BUILD_WITH_INSTALL_RPATH FALSE)
set(CMAKE_INSTALL_RPATH "\$ORIGIN/../lib")
set(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)

set(CMAKE_CC_FLAGS "${CMAKE_CC_FLAGS} -std=c++1z -O3 -fPIC -Wall")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++1z -O3 -fPIC -Wall")
file(GLOB_RECURSE SRC "src/*.cpp")
list(FILTER SRC EXCLUDE REGEX "main.cpp$")
file(GLOB_RECURSE HEADERS "src/*.h")
include_directories(src/ src/syzygy/)

add_library(stockfish SHARED ${SRC})

add_executable(main "src/main.cpp")
target_link_libraries(main stockfish pthread)

install(TARGETS stockfish
        RUNTIME DESTINATION bin
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib)
install(DIRECTORY src/ DESTINATION include FILES_MATCHING PATTERN "*.h")


install(TARGETS main
        RUNTIME DESTINATION bin
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib)

