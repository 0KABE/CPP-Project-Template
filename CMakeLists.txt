cmake_minimum_required(VERSION 3.0)
project(Cpp_Project_Template)

set(CMAKE_CXX_STANDARD 20)

add_executable(${PROJECT_NAME} main.cpp src/example.cc)

target_include_directories(${PROJECT_NAME} PRIVATE include)

# Conan Home Path
if (DEFINED ENV{CONAN_USER_HOME})
    message("Conan Home: $ENV{CONAN_USER_HOME}")
else ()
    message("Conan Home: Default conan home")
endif ()

include(cmake/conan.cmake)
conan_cmake_configure(
        REQUIRES
        gtest/1.10.0
        benchmark/1.8.0
        spdlog/1.9.2
        cli11/2.2.0
        boost/1.82.0

        GENERATORS
        CMakeDeps
)
conan_cmake_autodetect(settings)

conan_cmake_install(PATH_OR_REFERENCE .
        BUILD missing
        REMOTE conancenter
        SETTINGS ${settings}
)
