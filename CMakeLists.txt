cmake_minimum_required(VERSION 3.16)
project(2lbb-dy)
set(CMAKE_CXX_STANDARD 17)

find_package(OpenMP REQUIRED)

option(CXX "enable C++ compilation" ON)
if(CXX)
    enable_language(CXX)
endif()

set(Boost_ROOT "D:/Tools/boost_1_83_0")
set(Boost_LIBRARY_DIR "D:/Tools/boost_1_83_0/lib64-msvc-14.3")

set(ENV{XGB_HOME} "D:/p/opre/2023.0615-main/RouteOpt/Dependency/xgb")
set(ENV{GUROBI_HOME} "D:/工具/gurobi/win64")
set(ENV{CPLEX_HOME} "D:/工具/CPLEX")
set(ENV{CVRPSEP_HOME} "D:/p/opre/2023.0615-main/RouteOpt/Dependency/cvrpsep")
set(ENV{EIGEN_HOME} "D:/p/opre/2023.0615-main/RouteOpt/Dependency/eigen-3.4.0")
set(ENV{HGS_HOME} "D:/p/opre/2023.0615-main/RouteOpt/Dependency/hgs")

if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Release")
endif()
set(CMAKE_CXX_FLAGS_RELEASE "-O3")
message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")

set(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/package)
find_package(CPLEX REQUIRED)
find_package(Boost REQUIRED)
find_package(GUROBI REQUIRED)
find_package(XGB REQUIRED)
find_package(CVRPSEP REQUIRED)
find_package(HGS REQUIRED)

include_directories(
        ${Boost_INCLUDE_DIRS}
        ${GUROBI_INCLUDE_DIRS}
        ${CPLEX_INCLUDE_DIRS}
        ${XGB_INCLUDE_DIRS}
        ${CVRPSEP_INCLUDE_DIRS}
        ${HGS_INCLUDE_DIRS}
        $ENV{EIGEN_HOME}
)

# Automatically add all subdirectories containing header files
file(GLOB_RECURSE HEADER_LIST LIST_DIRECTORIES true "${PROJECT_SOURCE_DIR}/include/*.hpp")
foreach (HEADER ${HEADER_LIST})
    get_filename_component(HEADER_DIR ${HEADER} DIRECTORY)
    include_directories(${HEADER_DIR})
endforeach ()

file(GLOB_RECURSE SOURCES "${PROJECT_SOURCE_DIR}/src/*.cpp")

add_executable(${PROJECT_NAME} ${SOURCES})

target_link_libraries(${CMAKE_PROJECT_NAME}
        ${CPLEX_LIBRARIES}
        ${GUROBI_LIBRARIES}
        ${Boost_MATH_LIBRARY}
        ${XGB_LIBRARIES}
        ${CVRPSEP_LIBRARIES}
        ${HGS_LIBRARIES}
        OpenMP::OpenMP_CXX
)

if(${CMAKE_SOURCE_DIR} STREQUAL ${CMAKE_CURRENT_SOURCE_DIR})
    include(FeatureSummary)
    feature_summary(WHAT ALL)
endif()
