cmake_minimum_required(VERSION 3.11.3)
set(CMAKE_CXX_STANDARD 17)

# Set output directories to the build
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG ${CMAKE_BINARY_DIR})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE ${CMAKE_BINARY_DIR})

if(WIN32)
    set(CMAKE_TOOLCHAIN_FILE "$ENV{VCPKG_ROOT}/scripts/buildsystems/vcpkg.cmake")
else()
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17 -pthread")
endif()

project(traffic_sim)

if(DEFINED VCPKG_INSTALLED_DIR)
    set(OpenCV_DIR "${VCPKG_INSTALLED_DIR}/${VCPKG_TARGET_TRIPLET}/share/opencv4")
else()
    message(WARNING "VCPKG_INSTALLED_DIR not defined. OpenCV_DIR might need to be set manually.")
endif()

# Find opencv4 dependency
find_package(OpenCV 4.1 REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})
link_directories(${OpenCV_LIBRARY_DIRS})
add_definitions(${OpenCV_DEFINITIONS})

# Find sources
file(GLOB project_SRCS src/*.cpp)

# Create an executable
add_executable(traffic_sim ${project_SRCS})

# Link libraries
target_link_libraries(traffic_sim ${OpenCV_LIBRARIES})
