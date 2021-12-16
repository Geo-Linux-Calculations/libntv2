cmake_minimum_required(VERSION 3.0)
project(ntv2)
set(MAJOR_VERSION 0)
set(DATE_VERSION 20201216)
set(NTV2_VERSION ${MAJOR_VERSION}.${DATE_VERSION})
project(ntv2 VERSION ${NTV2_VERSION})


set(SOURCES ${PROJECT_SOURCE_DIR}/src/libntv2.c)
file(GLOB HEADERS ${PROJECT_SOURCE_DIR}/include/*.h)

add_library(ntv2 SHARED ${SOURCES})
set_target_properties(ntv2 PROPERTIES VERSION ${NTV2_VERSION} SOVERSION ${MAJOR_VERSION})
target_compile_options(ntv2 PRIVATE -DNTV2_NO_MUTEXES)
target_include_directories(ntv2 PRIVATE ${PROJECT_SOURCE_DIR}/include)
target_link_libraries(ntv2 m)
set_target_properties(ntv2 PROPERTIES PUBLIC_HEADER "${HEADERS}")

add_library(ntv2_static STATIC ${SOURCES})
target_compile_options(ntv2_static PRIVATE -DNTV2_NO_MUTEXES)
target_include_directories(ntv2_static PRIVATE ${PROJECT_SOURCE_DIR}/include)

add_executable(ntv2_cvt src/ntv2_cvt.c)
target_include_directories(ntv2_cvt PRIVATE ${PROJECT_SOURCE_DIR}/include)
target_link_libraries(ntv2_cvt ntv2)

add_executable(ntv2_file src/ntv2_file.c)
target_include_directories(ntv2_file PRIVATE ${PROJECT_SOURCE_DIR}/include)
target_link_libraries(ntv2_file ntv2)

install(TARGETS ntv2 ntv2_static DESTINATION ${CMAKE_INSTALL_PREFIX}/lib)
install(TARGETS ntv2_cvt ntv2_file DESTINATION ${CMAKE_INSTALL_PREFIX}/bin)
install(FILES ${HEADERS} DESTINATION ${CMAKE_INSTALL_PREFIX}/include)
install(FILES LICENSE README.md CONTENTS LICENSE DESTINATION ${CMAKE_INSTALL_PREFIX}/share/doc/libntv2)
