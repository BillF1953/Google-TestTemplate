cmake_minimum_required(VERSION 2.8.8)
project(gtest_builder C CXX) #allowed with C CXX
include(ExternalProject) #cmake module called external project adsa buhcn of commands

set(GTEST_FORCE_SHARED_CRT ON)    #Uses shared run-time lib even when Google Test is built as a static lib
set(GTEST_DISABLE_PTHREADS OFF) #turns of pthreads

if(MINGW)
    set(GTEST_DISABLE_PTHREADS ON) #turns them on for ming
endif()

ExternalProject_Add(googletest #googletest = NAME
        GIT_REPOSITORY https://github.com/google/googletest.git #CVS REPOSITORY
        CMAKE_ARGS -DCMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG:PATH=DebugLibs
        -DCMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE:PATH=ReleaseLibs
        -DCMAKE_CXX_FLAGS=${MSVC_COMPILER_DEFS}
        -Dgtest_force_shared_crt=${GTEST_FORCE_SHARED_CRT}
        -Dgtest_disable_pthreads=${GTEST_DISABLE_PTHREADS}
        -DBUILD_GTEST=ON
        PREFIX "${CMAKE_CURRENT_BINARY_DIR}"
        # Disable install step
        INSTALL_COMMAND   #Reenable to to install
        )

# Specify include dir
ExternalProject_Get_Property(googletest source_dir)
set(GTEST_INCLUDE_DIRS ${source_dir}/googletest/include PARENT_SCOPE)

# Specify MainTest's link libraries
ExternalProject_Get_Property(googletest binary_dir)
set(GTEST_LIBS_DIR ${binary_dir}/googlemock/gtest PARENT_SCOPE) #scopes to main


