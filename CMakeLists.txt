cmake_minimum_required(VERSION 3.16.3)
if (POLICY CMP0167)
    cmake_policy(SET CMP0167 NEW) # find Boost's own CMake config file
endif ()

project(mini-cpp-project)

set(CMAKE_CXX_STANDARD 20)

find_package(Boost REQUIRED OPTIONAL_COMPONENTS headers)
find_package(OpenSSL REQUIRED)

add_executable(${PROJECT_NAME} ${CMAKE_CURRENT_LIST_DIR}/src/main.cpp)
target_link_libraries(${PROJECT_NAME}
    PRIVATE
        ${Boost_LIBRARIES}
        OpenSSL::SSL
        OpenSSL::Crypto
)
target_compile_options(${PROJECT_NAME} PRIVATE $<$<BOOL:${MSVC}>:/bigobj>)

enable_testing()
add_test(NAME repro COMMAND ${PROJECT_NAME} 127.0.0.1 9050)
set_tests_properties(repro PROPERTIES TIMEOUT 60)
