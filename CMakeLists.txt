cmake_minimum_required(VERSION 3.21)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_OSX_DEPLOYMENT_TARGET "11.0" CACHE STRING "")
set(CMAKE_CXX_VISIBILITY_PRESET hidden)

project(UrlTrigger VERSION 1.0.0)

file(GLOB SOURCES src/*.cpp)
add_library(${PROJECT_NAME} SHARED ${SOURCES})

if (NOT DEFINED ENV{GEODE_SDK})
	message(FATAL_ERROR "Unable to find Geode SDK! Please define GEODE_SDK environment variable to point to Geode")
else()
	message(STATUS "Found Geode: $ENV{GEODE_SDK}")
endif()

add_subdirectory($ENV{GEODE_SDK} ${CMAKE_CURRENT_BINARY_DIR}/geode)

include(FetchContent)
FetchContent_Declare(
	gof
	GIT_REPOSITORY https://github.com/Bao-143/geode-game-objects-factory.git
	GIT_TAG main
	SOURCE_SUBDIR no-cmake-here
)
FetchContent_MakeAvailable(gof)

file(MAKE_DIRECTORY ${CMAKE_BINARY_DIR}/gof_include)
file(COPY ${gof_SOURCE_DIR}/include DESTINATION ${CMAKE_BINARY_DIR}/gof_include/user95401.game-objects-factory)
target_include_directories(${PROJECT_NAME} PRIVATE ${CMAKE_BINARY_DIR}/gof_include)

setup_geode_mod(${PROJECT_NAME})
