cmake_minimum_required(VERSION 3.15)

project(theseus LANGUAGES CXX VERSION 2.0.0)

# Get MFEM
find_package(MFEM 4.9 REQUIRED CONFIG) 

add_library(theseus)
target_compile_features(theseus PUBLIC cxx_std_20) # Require C++20
add_subdirectory(theseus)
target_sources(theseus
   PRIVATE 
      ${THESEUS_SOURCES}
   PUBLIC
      FILE_SET public_headers TYPE HEADERS
      FILES
         ${THESEUS_HEADERS})


# Install theseus
include(GNUInstallDirs)

message(STATUS "Install directory: ${CMAKE_INSTALL_PREFIX}")

# From: https://stackoverflow.com/questions/17511496/how-to-create-a-shared-library-with-cmake
install(TARGETS theseus
   EXPORT theseusTargets
   RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
   LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
   ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
   FILE_SET public_headers DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

install(EXPORT theseusTargets
   FILE "theseus-targets.cmake"
   DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/theseus)
   
include(CMakePackageConfigHelpers)
configure_package_config_file("cmake/theseus-config.cmake.in"
   "theseus-config.cmake"
   INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/theseus)

write_basic_package_version_file(
  ${CMAKE_CURRENT_BINARY_DIR}/theseus-config-version.cmake
  VERSION ${PROJECT_VERSION}
  COMPATIBILITY SameMajorVersion)

install(FILES 
   ${CMAKE_CURRENT_BINARY_DIR}/theseus-config.cmake
   ${CMAKE_CURRENT_BINARY_DIR}/theseus-config-version.cmake
   DESTINATION ${CMAKE_INSTALL_LIBDIR}/theseus)
