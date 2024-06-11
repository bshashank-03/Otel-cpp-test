cmake_minimum_required(VERSION 3.15)

set(CMAKE_CXX_STANDARD 17)

project(CrowSample)

# Add the executable
add_executable(CrowSample main.cpp)

set(INCLUDE_PATHS boost_1_85_0 ${CMAKE_SOURCE_DIR}/../crow/include)

set(OTEL_ROOT ${CMAKE_SOURCE_DIR}/../otel-cpp)

set(opentelemetry-cpp_DIR ${OTEL_ROOT}/lib/cmake/opentelemetry-cpp)

# Use find_package to include OpenTelemetry C++
find_package(opentelemetry-cpp CONFIG REQUIRED NO_DEFAULT_PATH)

include_directories(${CMAKE_SOURCE_DIR}/../opentelemetry-cpp/exporters/otlp/include)

if(WITH_OTLP_FILE)
    add_subdirectory(otlp)
endif()

# Include directories for Crow
target_include_directories(CrowSample PUBLIC ${INCLUDE_PATHS})

target_include_directories(CrowSample PRIVATE ${OPENTELEMETRY_CPP_INCLUDE_DIRS})

# Include directories for both Crow and opentelemetry-cpp
target_link_libraries(CrowSample PRIVATE ${OPENTELEMETRY_CPP_LIBRARIES})