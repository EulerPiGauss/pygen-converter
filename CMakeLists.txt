CMAKE_MINIMUM_REQUIRED(VERSION 3.0.2)
include(cmake/base.cmake)

set(PROJECT_NAME pygen)
set(PROJECT_DESCRIPTION "This is an header only library that provides list to eigen and numpy to eigen converters for boost python.")
set(PROJECT_URL https://github.com/vsamy/pygen-converter.git)

project(${PROJECT_NAME} CXX)

setup_project()

option(DISABLE_TESTS "Disable unit tests." OFF)

if(MSVC)
    set(CMAKE_MSVCIDE_RUN_PATH "\$(SolutionDir)/src/\$(Configuration)")
    add_compile_options("/bigobj")
endif(MSVC)

add_library(${PROJECT_NAME} INTERFACE)
target_include_directories(${PROJECT_NAME} INTERFACE
    $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include/${PROJECT_NAME}>
)
install(TARGETS ${PROJECT_NAME}
    EXPORT ${TARGETS_EXPORT_NAME}
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    INCLUDES DESTINATION ${INCLUDE_INSTALL_DIR}
)
install(FILES "include/converters.h" DESTINATION ${INCLUDE_INSTALL_DESTINATION})

if(NOT ${DISABLE_TESTS})
    add_subdirectory(tests)
endif()

setup_project_finalize()