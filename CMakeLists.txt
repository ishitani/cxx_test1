cmake_minimum_required(VERSION 3.14)
project(CXXTest1 CXX C)

SET(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake ${CMAKE_MODULE_PATH})

include(cmake/third-party.cmake)
set(CMAKE_CXX_STANDARD 17)

include(FetchContent)

# gtest
# get_third_party(gtest)
# add_subdirectory(${CMAKE_CURRENT_BINARY_DIR}/googletest-src
#   ${CMAKE_CURRENT_BINARY_DIR}/googletest-build
#   EXCLUDE_FROM_ALL)
FetchContent_Declare(
  googletest
  GIT_REPOSITORY https://github.com/google/googletest.git
  GIT_TAG release-1.12.1
  GIT_PROGRESS TRUE)
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)

# spdlog
# get_third_party(spdlog)
# include_directories(${CMAKE_CURRENT_BINARY_DIR}/spdlog/include)
# add_subdirectory(${CMAKE_CURRENT_BINARY_DIR}/spdlog ${CMAKE_CURRENT_BINARY_DIR}/spdlog-build)
FetchContent_Declare(
  spdlog
  GIT_REPOSITORY https://github.com/gabime/spdlog.git
  GIT_TAG v1.13.0
  GIT_PROGRESS TRUE)
FetchContent_MakeAvailable(spdlog)

add_executable(cxx_test1 main.cpp)
target_include_directories(cxx_test1 PUBLIC ${spdlog_SOURCE_DIR}/include)

set(PROJECT_NAME cxx_test1)
add_executable(${PROJECT_NAME}-googletest test_main.cpp)
target_link_libraries(${PROJECT_NAME}-googletest gtest_main)
target_include_directories(${PROJECT_NAME}-googletest PUBLIC ${googletest_SOURCE_DIR}/googletest/include)

add_test(NAME test COMMAND ${PROJECT_NAME}-googletest)
enable_testing()
