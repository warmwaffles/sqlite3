cmake_minimum_required(VERSION 3.2 FATAL_ERROR)

project(sqlite3 C)

if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Debug)
endif(NOT CMAKE_BUILD_TYPE)

include(FindPkgConfig)

find_package(Threads REQUIRED)

set(CMAKE_C_FLAGS_DEBUG "-std=c11 -Wall -Wextra -g")
set(CMAKE_C_FLAGS_RELEASE "-std=c11 -Wall -Wextra -O3 -flto")

if (CMAKE_BUILD_TYPE MATCHES Debug)
    add_definitions("-DDEBUG=1")
else()
    add_definitions("-DNDEBUG=1")
endif()

add_definitions("-D_GNU_SOURCE")

include_directories(include)

add_library(${PROJECT_NAME} STATIC src/sqlite3.c)
target_link_libraries(${PROJECT_NAME} PUBLIC Threads::Threads)

add_executable(${PROJECT_NAME}-cli src/shell.c)
target_link_libraries(${PROJECT_NAME}-cli PUBLIC ${PROJECT_NAME})

if(UNIX)
    target_link_libraries(${PROJECT_NAME} PUBLIC dl)
    target_link_libraries(${PROJECT_NAME}-cli PUBLIC dl)
endif()

target_include_directories(${PROJECT_NAME} PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
    PRIVATE src)

export(TARGETS ${PROJECT_NAME} FILE ${PROJECT_NAME}-config.cmake)
