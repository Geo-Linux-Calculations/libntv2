
cmake_minimum_required(VERSION 3.11)
project(ntv2)

set(SOURCES
  ${PROJECT_SOURCE_DIR}/src/libntv2.c
)

file(GLOB HEADERS ${PROJECT_SOURCE_DIR}/include/*.h)

add_library(ntv2 STATIC ${SOURCES})

target_compile_options(ntv2 PRIVATE
  -DNTV2_NO_MUTEXES
)

target_include_directories(ntv2 PRIVATE
  ${PROJECT_SOURCE_DIR}/include
)

set_target_properties(ntv2
  PROPERTIES
  PUBLIC_HEADER "${HEADERS}"
)

install(
  TARGETS ntv2
  PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_PREFIX}/include/ntv2
)
