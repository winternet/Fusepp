cmake_minimum_required(VERSION 3.9)

project(fusepp VERSION 3.0.0)

include(FindPkgConfig)

pkg_check_modules(FUSE REQUIRED fuse3)
if(FUSE_FOUND)
  message(STATUS "Found fuse (include: ${FUSE_INCLUDE_DIRS}, library: ${FUSE_LIBRARIES})")
endif()

add_library(${PROJECT_NAME} Fuse.cpp)
target_include_directories(${PROJECT_NAME} PRIVATE ${FUSE_INCLUDE_DIRS})

set_target_properties(${PROJECT_NAME} PROPERTIES 
  VERSION ${PROJECT_VERSION}
  PUBLIC_HEADER "Fuse.h;Fuse-impl.h"
  COMPILE_FLAGS "-D_FILE_OFFSET_BITS=64")

install(TARGETS ${PROJECT_NAME} 
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
