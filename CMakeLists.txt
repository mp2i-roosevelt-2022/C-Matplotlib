cmake_minimum_required(VERSION 3.20)

project(cgraph LANGUAGES C VERSION 0.1.0)

# include(CTest)
# enable_testing()

include(${CMAKE_SOURCE_DIR}/cmake/option.cmake)
include(${CMAKE_SOURCE_DIR}/cmake/CompilerWarnings.cmake)

set_option(BUILD_TEST OFF BOOL "Build test code for library")

set_choice (CMAKE_BUILD_TYPE "Debug;Release;MinSizeRel;RelWithDebInfo" "Debug" "Choose type of build")

set (SRCS

)

set (HEADERS

)

if (BUILD_TEST)
    add_subdirectory(${CMAKE_SOURCE_DIR}/test)
endif()

include_directories(${CMAKE_SOURCE_DIR}/include)

set (CMAKE_C_STANDARD 17)
set (CMAKE_C_STANDARD_REQUIRED ON)

add_library (${PROJECT_NAME} STATIC ${SRCS} ${HEADERS})
target_link_libraries(${PROJECT_NAME} PRIVATE m pthread)

foreach(header ${HEADERS})
    set_target_properties(${PROJECT_NAME} PROPERTIES PUBLIC_HEADER ${header})
endforeach()

set_target_warnings(${PROJECT_NAME})

include(GNUInstallDirs)

install(TARGETS ${PROJECT_NAME} ARCHIVE)
install(TARGETS ${PROJECT_NAME} PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR} )

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
set(CPACK_DEBIAN_PACKAGE_MAINTAINER "Quentin Horgues")
set(CPACK_DEB_COMPONENTS_INSTALL YES)
set(CPACK_DEBIAN_PACKAGE_DEPENDS "libsdl2-dev libsdl2-ttf-dev")
set(CPACK_DESKTOP_SHORTCUTS ON)
include(CPack)