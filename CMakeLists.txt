cmake_minimum_required(VERSION 2.8.3)

project(poco_vendor)

set(PACKAGE_VERSION "1.0.0")

list(INSERT CMAKE_MODULE_PATH 0 "${CMAKE_CURRENT_SOURCE_DIR}/cmake/Modules")
find_package(Poco COMPONENTS Foundation)

if(NOT Poco_FOUND)
  # If Poco was not found, extract and install the embedded copy of poco-1.6.1
  include(ExternalProject)
  ExternalProject_Add(poco-1.6.1
    URL "${CMAKE_CURRENT_SOURCE_DIR}/vendor/poco-poco-1.6.1-release-osrf-repack.tar.gz"
    URL_MD5 2fafcd519ab95eb9fb00c1232d646ba8
    CMAKE_ARGS
      -DENABLE_XML:BOOL=OFF
      -DENABLE_JSON:BOOL=OFF
      -DENABLE_MONGODB:BOOL=OFF
      -DENABLE_UTIL:BOOL=OFF
      -DENABLE_NET:BOOL=OFF
      -DENABLE_NETSSL:BOOL=OFF
      -DENABLE_CRYPTO:BOOL=OFF
      -DENABLE_DATA:BOOL=OFF
      -DENABLE_ZIP:BOOL=OFF
      -DENABLE_PAGECOMPILER:BOOL=OFF
      -DENABLE_PAGECOMPILER_FILE2PAGE:BOOL=OFF
      -DCMAKE_INSTALL_PREFIX=${CMAKE_CURRENT_BINARY_DIR}/poco_install
      -Wno-dev
  )

  # The external project will install to the build folder, but we'll install that on make install.
  install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/poco_install/ DESTINATION ${CMAKE_INSTALL_PREFIX})
endif()

install(DIRECTORY cmake DESTINATION share/${PROJECT_NAME})