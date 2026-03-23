cmake_minimum_required(VERSION 3.20)

project(crypto_arbitrage_detector VERSION 0.1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

add_library(crypto_arb
  src/crypto_arb.cpp
)

target_include_directories(crypto_arb
  PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

if(MSVC)
  target_compile_options(crypto_arb PRIVATE /W4 /permissive- /O2 /EHsc)
else()
  target_compile_options(crypto_arb PRIVATE -Wall -Wextra -Wpedantic -O3)
endif()

add_executable(crypto_arb_bench
  src/crypto_arb_main.cpp
)

target_link_libraries(crypto_arb_bench PRIVATE crypto_arb)
if(MSVC)
  target_compile_options(crypto_arb_bench PRIVATE /EHsc)
endif()

add_executable(crypto_arb_tests
  tests/crypto_arb_tests.cpp
)

target_link_libraries(crypto_arb_tests PRIVATE crypto_arb)
if(MSVC)
  target_compile_options(crypto_arb_tests PRIVATE /EHsc)
endif()
