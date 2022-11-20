#Reference: 
# https://doc.qt.io/qt-6/cmake-get-started.html

cmake_minimum_required(VERSION 3.20)

set(CMAKE_TOOLCHAIN_FILE qt6-mingw-x64.cmake)

project(main VERSION 1.0.0 LANGUAGES CXX)

# 查找 Qt6 的模块
find_package(Qt6 REQUIRED COMPONENTS Widgets)
qt_standard_project_setup()
 
aux_source_directory(. PROJECT_SRC_LIST)
add_executable(${PROJECT_NAME} ${PROJECT_SRC_LIST})

target_link_libraries(${PROJECT_NAME} 
    PRIVATE Qt6::Widgets
)

# 发布程序时不创建控制台窗口
if(CMAKE_BUILD_TYPE STREQUAL Release)
    set_target_properties(${PROJECT_NAME} PROPERTIES WIN32_EXECUTABLE ON)
endif()

# 将需要的动态库拷贝到输出目录
add_custom_command(
    TARGET  ${PROJECT_NAME}
    POST_BUILD COMMAND ${QT6_WINDEPLOY} --no-translations
    ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/${PROJECT_NAME}.exe)

