CMAKE_MINIMUM_REQUIRED(VERSION 2.6)
PROJECT(kick)

FIND_PACKAGE(PkgConfig REQUIRED)
SET(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${PROJECT_SOURCE_DIR}/modules")
FIND_PACKAGE(Mono REQUIRED)

OPTION(FORCE32 "Force a 32bit compile on 64bit" OFF)
if (APPLE)
    SET(CMAKE_OSX_DEPLOYMENT_TARGET 10.7)
elseif(FORCE32)
    SET(CMAKE_C_FLAGS "-m32")
    SET(CMAKE_CXX_FLAGS "-m32")
endif()

if (APPLE)
    SET(BIN_LIBROOT "osx")
    SET(CMAKE_EXECUTABLE_SUFFIX ".bin.osx")
    SET(CMAKE_EXE_LINKER_FLAGS "-framework Foundation")
    SET(KICKLIBS iconv)
    include_directories(${INCULDE_DIRECTORIES} /Library/Frameworks/Mono.framework/Headers/mono-2.0)
elseif (CMAKE_SIZEOF_VOID_P MATCHES "8" AND NOT(FORCE32))
    SET(BIN_LIBROOT "lib64")
    SET(CMAKE_EXECUTABLE_SUFFIX ".bin.x86_64")
    SET(BIN_RPATH "\$ORIGIN/lib64")
    SET(KICKLIBS m rt dl)
    include_directories(${INCULDE_DIRECTORIES} /usr/include/mono-2.0)
else()
    SET(BIN_LIBROOT "lib")
    if(${CMAKE_SYSTEM_PROCESSOR} MATCHES "arm") 
      SET(CMAKE_EXECUTABLE_SUFFIX ".bin.arm")
    else()
      SET(CMAKE_EXECUTABLE_SUFFIX ".bin.x86")
    endif()
    SET(BIN_RPATH "\$ORIGIN/lib")
    SET(KICKLIBS m rt dl)
    include_directories(${INCULDE_DIRECTORIES} /usr/include/mono-2.0)
endif()

ADD_DEFINITIONS(-DENABLE_BINRELOC)

set(CMAKE_SKIP_BUILD_RPATH TRUE)
set(CMAKE_BUILD_WITH_INSTALL_RPATH TRUE)
set(CMAKE_INSTALL_RPATH ${BIN_RPATH})
set(CMAKE_INSTALL_RPATH_USE_LINK_PATH FALSE)

SET(SOURCE
    kick.c
    binreloc.c
)

ADD_EXECUTABLE(kick
    ${SOURCE}
)

TARGET_LINK_LIBRARIES(kick
    ${CMAKE_SOURCE_DIR}/libmonosgen-2.0.a
    ${KICKLIBS}
    pthread
)
