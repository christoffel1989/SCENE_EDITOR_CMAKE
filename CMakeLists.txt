#cmake version
cmake_minimum_required(VERSION 3.1.0)

#project name
project(SCENE_EDITOR LANGUAGES CXX)

#import 3rd library
find_package(Qt5 REQUIRED COMPONENTS Widgets OpenGL)
find_package(OpenGL REQUIRED)

file(GLOB tool_header_files src/tool/*.h)
file(GLOB tool_source_files src/tool/*.cpp)
source_group("Header Files\\tool" FILES ${tool_header_files})
source_group("Source Files\\tool" FILES ${tool_source_files})

file(GLOB ui_header_files src/ui/*.h)
file(GLOB ui_source_files src/ui/*.cpp)
source_group("Header Files\\ui" FILES ${ui_header_files})
source_group("Source Files\\ui" FILES ${ui_source_files})

file(GLOB resource_files resource/*.qrc)
source_group("Resource Files" FILES ${resource_files})

add_executable(${PROJECT_NAME}
               src/main.cpp
               ${tool_header_files}
               ${tool_source_files}
               ${ui_header_files}
               ${ui_source_files}
               ${resource_files})

target_compile_features(${PROJECT_NAME} PRIVATE cxx_std_17)
set_target_properties(${PROJECT_NAME} PROPERTIES AUTOMOC ON AUTORCC ON)
target_include_directories(${PROJECT_NAME} 
                           PRIVATE
                           externals/assimp/include
						   externals/glm/include
						   externals/stbimage/include
                           src/tool
                           src/ui)

#lib
file(GLOB assimp_lib_files externals/assimp/lib/*.lib)
target_link_libraries(${PROJECT_NAME}
                      PRIVATE
                      Qt5::Widgets
                      Qt5::OpenGL
                      ${OPENGL_gl_LIBRARY}
                      ${assimp_lib_files})

#multi-core compile, use iso c++17 and close specified warning
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_FLAGS "/MP")

#bin path
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_CURRENT_SOURCE_DIR}/bin)
set_property(TARGET ${PROJECT_NAME} PROPERTY VS_DEBUGGER_WORKING_DIRECTORY "${CMAKE_SOURCE_DIR}/bin/$<CONFIGURATION>")

#creat project folder
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

#copy dll
set(EXTERNALS_ROOT_DIR "${CMAKE_CURRENT_SOURCE_DIR}/externals")
add_custom_command(TARGET ${PROJECT_NAME} PRE_BUILD COMMAND xcopy /d /f /y "\"${EXTERNALS_ROOT_DIR}/assimp/runtime/*.dll\"" "\"$<TARGET_FILE_DIR:${PROJECT_NAME}>\"")