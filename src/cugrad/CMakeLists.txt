############################################################
#
# $Id$
#
# Copyright (c) SCli 2020
#
# AUTHOR:
# Sylvain Prigent
# 

# Project name
project(cugrad LANGUAGES CUDA CXX)

## #################################################################
## Define files
## #################################################################

SET(SRC_FILES
  cugrad.cu
  cufft2d.cu
)

SET(HEADERS_FILES
  cugrad.h
  cufft2d.h
)
  
## #################################################################
## Library target
## #################################################################

add_library(${PROJECT_NAME} ${SRC_FILES} ${HEADERS_FILES})

target_link_libraries(${PROJECT_NAME} cufft ${SL_LIBRARIES})
target_compile_features(${PROJECT_NAME} PUBLIC cxx_std_11)
set_target_properties( ${PROJECT_NAME}
                       PROPERTIES CUDA_SEPARABLE_COMPILATION ON)

## #################################################################
## Export header file
## #################################################################

set(CMAKE_CXX_VISIBILITY_PRESET hidden)
set(CMAKE_VISIBILITY_INLINES_HIDDEN 1)

generate_export_header(${PROJECT_NAME}
  EXPORT_FILE_NAME "${PROJECT_NAME}Export.h")

add_custom_command(TARGET ${PROJECT_NAME} PRE_BUILD
  COMMAND ${CMAKE_COMMAND} ARGS -E copy_if_different "${${PROJECT_NAME}_BINARY_DIR}/${PROJECT_NAME}Export.h" "${CMAKE_BINARY_DIR}"
  COMMAND ${CMAKE_COMMAND} ARGS -E copy_if_different "${${PROJECT_NAME}_BINARY_DIR}/${PROJECT_NAME}Export.h" "${CMAKE_BINARY_DIR}/${PROJECT_NAME}Export")
