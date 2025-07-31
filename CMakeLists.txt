cmake_minimum_required(VERSION 3.11.0)

# TODO: Write proper build system :)
project(pizzas)

set(SRC_LIST
    src/Funghi.cpp
    src/Margherita.cpp
    src/Pizza.cpp
    src/Pizzeria.cpp
)

add_library(${PROJECT_NAME}-lib STATIC ${SRC_LIST})
target_compile_features(${PROJECT_NAME}-lib PRIVATE cxx_std_17)

add_executable(${PROJECT_NAME} src/main.cpp)
target_compile_features(${PROJECT_NAME} PRIVATE cxx_std_17)
target_link_libraries(${PROJECT_NAME} ${PROJECT_NAME}-lib pthread)


include(FetchContent)
FetchContent_Declare(
  googletest
  GIT_REPOSITORY https://github.com/google/googletest.git
  GIT_TAG        main # release-1.10.0
)

# For Windows: Prevent overriding the parent project's compiler/linker settings
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)


enable_testing()
add_executable(${PROJECT_NAME}-ut test/PizzeriaTest.cpp test/test_main.cpp)
target_include_directories(${PROJECT_NAME}-ut
    PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/src/
)
target_link_libraries(${PROJECT_NAME}-ut ${PROJECT_NAME}-lib pthread gtest_main gmock_main)

include(GoogleTest)
gtest_discover_tests(${PROJECT_NAME}-ut)
