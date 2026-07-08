cmake_minimum_required(VERSION 3.15)
project(SanrakshanEngine)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Threads REQUIRED)

# Use absolute paths to be safe
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)
include_directories(/usr/include) # For Asio

add_executable(sanrakshan_server 
    src/main.cpp
    src/NetworkGraph.cpp     
    src/SimulationEngine.cpp 
    src/WebServer.cpp        
)

target_link_libraries(sanrakshan_server PRIVATE Threads::Threads)