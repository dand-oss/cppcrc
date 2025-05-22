cmake_minimum_required(VERSION 3.14)

project(
    cppcrc
    VERSION 1.3.0
    LANGUAGES CXX
    DESCRIPTION "A very small, fast, header-only, C++ library for generating CRCs"
)

###############
# ASI default settings
list(APPEND CMAKE_MODULE_PATH "$ENV{ASV_CMAKE}")
include(asv_cmake_defaults)
asv_cmake_defaults( )
###############

set(libname cppcrc)
add_library(${libname} INTERFACE)
add_library(${libname}::${libname} ALIAS ${libname})

include(GNUInstallDirs)

target_include_directories(${libname} INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:include>
)

# To include cppcrc in your project, just add the following two lines to your CMakeLists.txt file
#   add_subdirectory(cppcrc/)
#   target_link_libraries(your_project_name cppcrc)

install(TARGETS ${libname}
        EXPORT ${libname}_Targets
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    "${libname}ConfigVersion.cmake"
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY SameMajorVersion)

configure_package_config_file(
  "${PROJECT_SOURCE_DIR}/cmake/${libname}Config.cmake.in"
  "${PROJECT_BINARY_DIR}/${libname}Config.cmake"
  INSTALL_DESTINATION
  ${CMAKE_INSTALL_DATAROOTDIR}/${libname}/cmake)

install(EXPORT ${libname}_Targets
    FILE ${libname}Targets.cmake
    NAMESPACE ${libname}::
    DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/${libname}/cmake)

install(FILES "${PROJECT_BINARY_DIR}/${libname}Config.cmake"
    "${PROJECT_BINARY_DIR}/${libname}ConfigVersion.cmake"
    DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/${libname}/cmake)

install(FILES
    ${PROJECT_SOURCE_DIR}/cppcrc.h
    DESTINATION include)

set(CPACK_RESOURCE_FILE_LICENSE "${PROJECT_SOURCE_DIR}/LICENSE")

include(CPack)
