cmake_minimum_required(VERSION 3.20.0 FATAL_ERROR)

project(VolumeRendering)

# Add all Source Files
file(GLOB_RECURSE SOURCE_FILES
	${CMAKE_SOURCE_DIR}/src/*.cpp
)

# Add all Header Files
file(GLOB_RECURSE HEADER_FILES
	${CMAKE_SOURCE_DIR}/src/*.h
	${CMAKE_SOURCE_DIR}/src/*.hpp
)

# Copy shaders to output directory so they can be loaded and compiled
file(COPY ${CMAKE_SOURCE_DIR}/src/shaders
	DESTINATION ${CMAKE_CURRENT_BINARY_DIR})

add_executable(${PROJECT_NAME} ${HEADER_FILES} ${SOURCE_FILES})

# Find the Required Packages (if Any -> OpenGL/Vulkan or Any)
find_package(OpenGL REQUIRED)

# Variable for the Libs to add to the Linkers
set(LIBS glfw opengl32 glad glm)

# Variables for Paths of External Libraries
set(GLFW_ROOT_DIR external/glfw)
set(GLAD_ROOT_DIR external/glad)
set(GLM_ROOT_DIR external/glm)

# Add the External Libraries / Files Directory
add_subdirectory(${GLFW_ROOT_DIR})
add_subdirectory(${GLAD_ROOT_DIR})
add_subdirectory(${GLM_ROOT_DIR})

# Add the Include Directories for the Libraries / Files
# Used for adding #include into the cpp files

# Include Files From Within the Workspace Folder
target_include_directories(${PROJECT_NAME}
	PUBLIC ${GLFW_ROOT_DIR}/include
	PUBLIC ${GLAD_ROOT_DIR}/include/glad
	PUBLIC ${GLM_ROOT_DIR}
	PUBLIC ${SHADERS_ROOT_DIR}
)

# Include Files From Within the Workspace Folder
target_link_directories(${PROJECT_NAME}
	PRIVATE ${GLFW_ROOT_DIR}/src
	PRIVATE ${GLAD_ROOT_DIR}/src
)

# Add the Libs to the Linker for the Project
target_link_libraries(${PROJECT_NAME} ${LIBS})