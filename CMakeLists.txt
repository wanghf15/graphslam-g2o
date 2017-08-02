cmake_minimum_required(VERSION 2.8)
INCLUDE_DIRECTORIES(${CSPARSE_INCLUDE_DIR})

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)

#设定.cmake文件路径
#SET(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "{CMAKE_CURRENT_SOURCE_DIR}/cmake_modules")
LIST(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake_modules)
#ADD_SUBDIRECTORY(${PROJECT_SOURCE_DIR}/src)

ADD_LIBRARY(tutorial_slam2d_library ${G2O_LIB_TYPE}
  edge_se2_pointxy.cpp  se2.h                vertex_point_xy.h
  edge_se2.cpp    edge_se2_pointxy.h    vertex_se2.cpp
  edge_se2.h      rand.h                vertex_point_xy.cpp  vertex_se2.h
  parameter_se2_offset.h parameter_se2_offset.cpp
  types_tutorial_slam2d.h types_tutorial_slam2d.cpp
  simulator.h simulator.cpp
  g2o_tutorial_slam2d_api.h
)

SET(G2O_LIBS g2o_cli g2o_ext_freeglut_minimal g2o_simulator g2o_solver_slam2d_linear g2o_types_icp g2o_types_slam2d g2o_core g2o_interface g2o_solver_csparse g2o_solver_structure_only g2o_types_sba g2o_types_slam3d g2o_csparse_extension g2o_opengl_helper g2o_solver_dense g2o_stuff g2o_types_sclam2d g2o_parser g2o_solver_pcg g2o_types_data g2o_types_sim3 cxsparse tutorial_slam2d_library)

find_package(Eigen3 REQUIRED)
find_package(CSparse REQUIRED)
find_package(Cholmod REQUIRED)
include_directories(${CHOLMOD_INCLUDE_DIR})
include_directories(${CSPARSE_INCLUDE_DIR})
include_directories(${EIGEN3_INCLUDE_DIR})

if(CMAKE_COMPILER_IS_GNUCXX)
  set(CMAKE_CXX_FLAGS "-std=c++11 ${CMAKE_CXX_FLAGS}")
  message(STATUS "optional:-std=c++11")
endif(CMAKE_COMPILER_IS_GNUCXX)

SET_TARGET_PROPERTIES(tutorial_slam2d_library PROPERTIES OUTPUT_NAME ${LIB_PREFIX}tutorial_slam2d)

LINK_LIBRARIES(${PROJECT_SOURCE_DIR}/bin)

#TARGET_LINK_LIBRARIES(tutorial_slam2d_library core solver_csparse)

ADD_EXECUTABLE(tutorial_slam2d
  tutorial_slam2d.cpp
)

SET_TARGET_PROPERTIES(tutorial_slam2d PROPERTIES OUTPUT_NAME tutorial_slam2d${EXE_POSTFIX})

#TARGET_LINK_LIBRARIES(tutorial_slam2d tutorial_slam2d_library)
target_link_libraries(tutorial_slam2d ${G2O_LIBS})
