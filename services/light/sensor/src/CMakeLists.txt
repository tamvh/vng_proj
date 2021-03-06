#*******************************************************************************
#  Copyright (c) 2016 VNG Corp. iot lab
# 
#  Contributors:
#     Trung Bui Quang(trungbq#@vng.com.vn) - initial version
#*******************************************************************************/

# Note: on OS X you should install XCode and the associated command-line tools
SET(PROJECT_NAME "vng.iot.services.light.sensor")
PROJECT("${PROJECT_NAME}" CXX)
set (CMAKE_CXX_FLAGS "--std=gnu++11 ${CMAKE_CXX_FLAGS}")
CMAKE_MINIMUM_REQUIRED(VERSION 3.0)

## build settings
SET(VNGIOTSERVICESLIGHTSENSOR_VERSION_MAJOR 1)
SET(VNGIOTSERVICESLIGHTSENSOR_VERSION_MINOR 0)
SET(VNGIOTSERVICESLIGHTSENSOR_VERSION_PATCH 0)
SET(VNGIOTSERVICESLIGHTSENSOR_VERSION ${VNGIOTSERVICESLIGHTSENSOR_VERSION_MAJOR}.${VNGIOTSERVICESLIGHTSENSOR_VERSION_MINOR}.${VNGIOTSERVICESLIGHTSENSOR_VERSION_PATCH})

EXECUTE_PROCESS(COMMAND date -u OUTPUT_VARIABLE BUILD_TIMESTAMP)
STRING(STRIP ${BUILD_TIMESTAMP} BUILD_TIMESTAMP)

## build options
SET(VNGIOTSERVICESLIGHTSENSOR_WITH_SSL FALSE CACHE BOOL "Flag that defines whether to build ssl-enabled binaries too. ")
SET(VNGIOTSERVICESLIGHTSENSOR_BUILD_DOCUMENTATION FALSE CACHE BOOL "Create and install the HTML based API documentation (requires Doxygen)")
SET(VNGIOTSERVICESLIGHTSENSOR_BUILD_SAMPLES FALSE CACHE BOOL "Build sample programs")

include_directories(${CMAKE_CURRENT_SOURCE_DIR})
add_definitions(-DUSE_EPOLL -DUSE_LIBJSONC -DWITH_POSIX -DDEBUG_PRINT)
add_executable("${PROJECT_NAME}"
               application.cpp
               zcloudclient.cpp
               httpclient.cpp
               main.cpp)
SET_TARGET_PROPERTIES("${PROJECT_NAME}" PROPERTIES COMPILE_FLAGS "-I$ENV{STAGING_DIR}/usr/include/vng/iot/sdk")
TARGET_LINK_LIBRARIES("${PROJECT_NAME}" vng.iot.sdk curl)
SET(CPACK_PACKAGE_VERSION_MAJOR ${VNGIOTSERVICESLIGHTSENSOR_VERSION_MAJOR})
SET(CPACK_PACKAGE_VERSION_MINOR ${VNGIOTSERVICESLIGHTSENSOR_VERSION_MINOR})
SET(CPACK_PACKAGE_VERSION_PATCH ${VNGIOTSERVICESLIGHTSENSOR_VERSION_PATCH})
SET_TARGET_PROPERTIES("${PROJECT_NAME}"
	PROPERTIES
        VERSION ${VNGIOTSERVICESLIGHTSENSOR_VERSION}
        SOVERSION ${VNGIOTSERVICESLIGHTSENSOR_VERSION_MAJOR})
    
INSTALL(TARGETS "${PROJECT_NAME}"
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib)

INCLUDE(CPack)
