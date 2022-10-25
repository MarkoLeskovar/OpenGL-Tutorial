cmake_minimum_required(VERSION 3.20.0 FATAL_ERROR)

project(OpenGL-Tutorial)

# Set C++ standard
set(CMAKE_CXX_STANDARD 11)

# Add all Source Files
# set(SOURCE_FILES main.cpp)

# Add all Header Files
set(HEADER_FILES shader.h stb_image.h)

# Find the Required Packages
find_package(OpenGL REQUIRED)
find_package(OpenMP REQUIRED)

# Variables for Paths of External Libraries
# Add external modules
set(GLFW_ROOT_DIR ${CMAKE_SOURCE_DIR}/external/glfw)
set(GLAD_ROOT_DIR ${CMAKE_SOURCE_DIR}/external/glad)
set(EIGEN_ROOT_DIR ${CMAKE_SOURCE_DIR}/external/eigen)
set(PYBIND_ROOT_DIR ${CMAKE_SOURCE_DIR}/external/pybind11)

# Add the External Libraries with active CMake files
add_subdirectory(${GLFW_ROOT_DIR})
add_subdirectory(${GLAD_ROOT_DIR})
add_subdirectory(${PYBIND_ROOT_DIR})

# Include directories of that files
include_directories(${GLFW_ROOT_DIR}/include)
include_directories(${EIGEN_ROOT_DIR})

# Variable for the Libs to add to the Linkers
set(LIBS glfw ${GLFW_LIBRARIES}
		 glad 
		 OpenMP::OpenMP_CXX
		 OpenGL::GL)


# Build flags for GLFW
set(BUILD_SHARED_LIBS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)



# Add main executable file
add_executable(${PROJECT_NAME} main.cpp ${HEADER_FILES})

# Add the Include Directories for Libraries or Files. Used for adding #include into the cpp files
target_include_directories(${PROJECT_NAME} 
	PUBLIC ${GLFW_ROOT_DIR}/include
	PUBLIC ${GLAD_ROOT_DIR}/include/glad
	PUBLIC ${PYBIND_ROOT_DIR}/include/pybind11
)

# Add the Link Directories for Libraries or Files. Used for creation of Static/Dynamic Libraries
target_link_directories(${PROJECT_NAME} 
	PRIVATE ${GLFW_ROOT_DIR}
	PRIVATE ${GLAD_ROOT_DIR}/src
	PRIVATE ${PYBIND_ROOT_DIR}
)

# Add the Libs to the Linker for the Project
target_link_libraries(${PROJECT_NAME} PRIVATE ${LIBS})


# EXPLANATION FOR SOME STUFF

# GLOB_RECURSE -> Recursively go through all subdirectories
#		of the given path and get all the files matching the extension provided
# file() -> Store the Files matching the RegEx into a variable
# set() -> Alternative to file() if you want to set variables for a custom behaviour