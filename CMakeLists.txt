cmake_minimum_required(VERSION 3.10)
project(libela VERSION 0.3 DESCRIPTION "Event loop abstraction library" LANGUAGES C)

# Compiler settings
add_compile_options(
    -Wstrict-prototypes 
    -Wmissing-prototypes 
    -Wno-unused-parameter
)

# Find dependencies
find_package(Libevent REQUIRED)

# Include directories
include_directories(include)

# Source files
set(SOURCE_FILES src/ela.c src/ela_libevent.c)

# Define the library target
add_library(ela STATIC ${SOURCE_FILES})
target_link_libraries(ela PUBLIC Libevent::Libevent)
set_target_properties(ela PROPERTIES PUBLIC_HEADER include/ela.h)

# Installation rules
install(TARGETS ela
    ARCHIVE DESTINATION lib
    PUBLIC_HEADER DESTINATION include)

# Optionally create a pkg-config file
include(FindPkgConfig)
if(PKG_CONFIG_FOUND)
    configure_file(${CMAKE_SOURCE_DIR}/ela.pc.in ${CMAKE_BINARY_DIR}/ela.pc @ONLY)
    install(FILES ${CMAKE_BINARY_DIR}/ela.pc DESTINATION lib/pkgconfig)
endif()
