cmake_minimum_required(VERSION 3.20)

project(Blackhole)

file(GLOB SRC_FILES
  "${PROJECT_SOURCE_DIR}/src/*.h"
  "${PROJECT_SOURCE_DIR}/src/*.cpp"
  "${PROJECT_SOURCE_DIR}/src/*.c"
  "${PROJECT_SOURCE_DIR}/src/*.cc")

add_executable(${CMAKE_PROJECT_NAME} ${SRC_FILES})

find_package(imgui REQUIRED)
find_package(glfw3 REQUIRED)
find_package(GLEW REQUIRED)
find_package(glm REQUIRED)

target_link_libraries(${CMAKE_PROJECT_NAME} PRIVATE imgui::imgui)
target_link_libraries(${CMAKE_PROJECT_NAME} PRIVATE glfw)
target_link_libraries(${CMAKE_PROJECT_NAME} PRIVATE GLEW::GLEW)
target_link_libraries(${CMAKE_PROJECT_NAME} PRIVATE glm::glm)

#find_package(stb REQUIRED)
#target_include_directories(${CMAKE_PROJECT_NAME} PRIVATE ${STB_INCLUDE_DIRS})

target_compile_features(${CMAKE_PROJECT_NAME} PRIVATE cxx_std_17)

# Copy assets files after build.
add_custom_command(
  TARGET ${CMAKE_PROJECT_NAME}
  POST_BUILD
  COMMAND ${CMAKE_COMMAND} -E copy_directory "${PROJECT_SOURCE_DIR}/assets"
          "$<TARGET_FILE_DIR:${CMAKE_PROJECT_NAME}>/assets")

add_custom_command(
  TARGET ${CMAKE_PROJECT_NAME}
  POST_BUILD
  COMMAND ${CMAKE_COMMAND} -E copy_directory "${PROJECT_SOURCE_DIR}/shader/"
          "$<TARGET_FILE_DIR:${CMAKE_PROJECT_NAME}>/shader")
