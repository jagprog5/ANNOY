cmake_minimum_required(VERSION 3.7)
set(CMAKE_CXX_STANDARD 17)
add_compile_options(-Wall -Wextra -Wpedantic -O1)
project(annoy_jagprog5)

add_library(${PROJECT_NAME}_lib INTERFACE)
target_include_directories(${PROJECT_NAME}_lib INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/include)

if(BUILD_TESTING)
  # fuzzer
  add_executable(fuzz_annoy src/fuzz.cpp)
  target_link_libraries(fuzz_annoy PRIVATE ${PROJECT_NAME}_lib)
  
  # unit tests
  set(BOOST_MIN_VERSION "1.63.0")
  add_executable(test_annoy src/test.cpp)
  find_package(Boost REQUIRED COMPONENTS unit_test_framework)
  target_link_libraries(test_annoy PRIVATE ${PROJECT_NAME}_lib ${Boost_UNIT_TEST_FRAMEWORK_LIBRARY})
  
endif()