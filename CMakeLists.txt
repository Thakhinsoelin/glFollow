cmake_minimum_required(VERSION 3.24...3.30) # FetchContent requires 3.11+
project(openGLTutorial)

include(FetchContent)


set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Define the external project
FetchContent_Declare(
  glfw
  GIT_REPOSITORY https://github.com/glfw/glfw.git
  GIT_BRANCH master
  #GIT_TAG        v1.11.0 # Or any specific tag/commit
)

# Fetch the project (download and configure)
FetchContent_MakeAvailable(glfw)
set(glew-cmake_BUILD_STATIC TRUE)
FetchContent_Declare(
  glew
  GIT_REPOSITORY https://github.com/Perlmint/glew-cmake.git
  GIT_BRANCH glew-cmake-release
  #GIT_TAG        v1.11.0 # Or any specific tag/commit
)

# Fetch the project (download and configure)
FetchContent_MakeAvailable(glew)

# Now you can use googletest in your project
message(STATUS "GLEW_LIBRARY: ${GLEW_LIBRARY}")
# Example: Create a simple test executable (requires googletest)
add_executable(${PROJECT_NAME})
add_subdirectory(src)

set_target_properties(${PROJECT_NAME} PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/${PROJECT_NAME})

set_property(TARGET ${PROJECT_NAME} PROPERTY VS_DEBUGGER_WORKING_DIRECTORY $<TARGET_FILE_DIR:${PROJECT_NAME}>)


add_custom_command(
        TARGET ${PROJECT_NAME} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_directory ${CMAKE_SOURCE_DIR}/src/resources $<TARGET_FILE_DIR:${PROJECT_NAME}>/resources
    )
    #DEPENDS ${PROJECT_NAME}

# Link against googletest libraries
find_package(OpenGL REQUIRED) # Add this line

target_link_libraries(${PROJECT_NAME} PRIVATE glfw $<TARGET_FILE:libglew_static> OpenGL::GL) #Add opengl to linking.
target_include_directories(${PROJECT_NAME} PRIVATE ${glew_SOURCE_DIR}/include)
# target_link_libraries(${PROJECT_NAME} glfw)
