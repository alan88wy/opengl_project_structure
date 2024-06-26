cmake_minimum_required( VERSION 3.3)
set(cmake_cxx_standard 17)
set(cmake_c_standard 99)
set(this triangle)

list(APPEND CMAKE_PREFIX_PATH "/usr/lib")

project(${this} CXX C)

option(GLFW_BUILD_DOCS OFF)
option(GLFW_BUILD_EXAMPLES OFF)
option(GLFW_BUILD_TESTS OFF)
add_subdirectory(Vendors/glfw)

# option(ASSIMP_BUILD_ASSIMP_TOOLS OFF)
# option(ASSIMP_BUILD_SAMPLES OFF)
# option(ASSIMP_BUILD_TESTS OFF)
# add_subdirectory(Vendors/assimp)

# option(BUILD_BULLET2_DEMOS OFF)
# option(BUILD_CPU_DEMOS OFF)
# option(BUILD_EXTRAS OFF)
# option(BUILD_OPENGL3_DEMOS OFF)
# option(BUILD_UNIT_TESTS OFF)
# add_subdirectory(Vendors/bullet)
# add_subdirectory(Vendors/glad) # Only add this if we include source file and CMakeList.txt

if(MSVC)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W4")
else()
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wpedantic -std=c++17")
    if(NOT WIN32)
        set(GLAD_LIBRARIES dl)
    endif()
endif()

include_directories(Headers/
                    # Vendors/assimp/include/
                    # Vendors/bullet/src/
		    Vendors/glad/include/
                    Vendors/glfw/include/
                    # Vendors/glm/
                    # Vendors/stb/
)

file(GLOB VENDORS_SOURCES Vendors/glad/glad.c)
file(GLOB PROJECT_HEADERS Headers/*.hpp
                          Headers/*.h)
file(GLOB PROJECT_SOURCES Sources/*.cpp)
# file(GLOB PROJECT_SHADERS Shaders/*.comp
#                           Shaders/*.frag
#                           Shaders/*.geom
#                           Shaders/*.vert)
file(GLOB PROJECT_CONFIGS CMakeLists.txt
                          Readme.md
                         .gitattributes
                         .gitignore
                         .gitmodules)

source_group("Headers" FILES ${PROJECT_HEADERS})
# source_group("Shaders" FILES ${PROJECT_SHADERS})
source_group("Sources" FILES ${PROJECT_SOURCES})
source_group("Vendors" FILES ${VENDORS_SOURCES})

add_definitions(-DGLFW_INCLUDE_NONE
                -DPROJECT_SOURCE_DIR=\"${PROJECT_SOURCE_DIR}\")
add_executable(${PROJECT_NAME} ${PROJECT_SOURCES} ${PROJECT_HEADERS}
	                       # ${PROJECT_SHADERS} ${PROJECT_CONFIGS}
                               ${PROJECT_CONFIGS}
                               ${VENDORS_SOURCES})
target_link_libraries(${PROJECT_NAME} glfw
                      ${GLFW_LIBRARIES} ${GLAD_LIBRARIES}
)
# target_link_libraries(${PROJECT_NAME} assimp glfw
#                       ${GLFW_LIBRARIES} ${GLAD_LIBRARIES}
#                       BulletDynamics BulletCollision LinearMath)
set_target_properties(${PROJECT_NAME} PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/${PROJECT_NAME})

# add_custom_command(
#     TARGET ${PROJECT_NAME} POST_BUILD
#     COMMAND ${CMAKE_COMMAND} -E copy_directory ${CMAKE_SOURCE_DIR}/Shaders $<TARGET_FILE_DIR:${PROJECT_NAME}>
#     )
# add_custom_command(
    # TARGET ${PROJECT_NAME} POST_BUILD
    # COMMAND ${CMAKE_COMMAND} -E copy_directory ${CMAKE_SOURCE_DIR}/Shaders $<TARGET_FILE_DIR:${PROJECT_NAME}>
    # DEPENDS ${PROJECT_SHADERS})
