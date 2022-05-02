cmake_minimum_required(VERSION 3.16)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include("cmake/HunterGate.cmake")

HunterGate(
        URL "https://github.com/sasha16613/hunter/archive/hunter.tar.gz"
        SHA1 "23610599545c7683dd111002cd31734c835e1f43"
)

project(project)

hunter_add_package(print)
find_package(print)

add_executable(${PROJECT_NAME} main.cpp)
target_link_libraries(${PROJECT_NAME} print)

install(TARGETS ${PROJECT_NAME} RUNTIME DESTINATION bin)
