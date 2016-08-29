cmake_minimum_required(VERSION 2.8.8)  #version
#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++0x")
set(PROJECT_NAME_STR Euler_w_GTEST) #assign value unset unassign format set(VAR VAL)  -advancae Parent_scope / Cache Entry /EVN variabble
project(${PROJECT_NAME_STR} C CXX)   #project_name variables  also sets Project_Source_dir, <project-Name>_Source_Dire Binary directory C CXX which languages your product supports
#Project_Source_Dir which is name in this example "google-test-examples_SOURCE_DIR" && "google-test-examples_BINARY_DIR" - top level sournce directory for the current project source directoy of the most recent project() command
#add_subdirectory
#project_binary_dir - full path to build directory fro the project
#if cmake_project_<project_name>_include the file reference will be included


find_package(Threads REQUIRED) # find and loads setting from an external project.  "Threads_FOUND" will be created as a boolean to tell if package was ofound. If found package info provided through new variables
#Imported Targets
#REQUIRED - stops processing with an error message if not found
#QUIET - disables error message
#MODUEL - advanced feature
#HOW IT FINDS PACKAGES
#Module Mode - searches for a file called "FindThreads.cmake in the CMAKE_MODULE_PATH (Installed under clion)  CMAKE auto imports
#if that doesn't work, STEP TWO config (advanced option)s  config looks for a config file provided by package. A cache entry called Threads_DIR to hold the firle.  You can override name of to search for new file
# one it finds it creates a configuratuion file who's address is stored in the cmake variable Threads_Conf. All files searchs Threads_CONSIDERED_CONFIGS. If not found theis will generate an erro.
#versional is The package configuration file may set <package>_FOUND to false to tell find_package that component requirements are not satisfied.

if ( CMAKE_COMPILER_IS_GNUCXX ) #this is a cmake variable that checks the compile for g++
    add_definitions(-Wall -ansi -Wno-deprecated -pthread -std=gnu++0x) #wall turns on a bunch of warning flags #warning #no deprecated warnings  pthread- portable  pthread - links a pthread librariry
    #adds preprocessor flags line for targets and current drietoy and below.
    #FLage beginning in -D or /D are added the COMPILE_DEFINITIONS directory proeprty for currently directory
    #
endif ()

if ( MSVC )
    #vc 2012 fix for vararg templates
    set(MSVC_COMPILER_DEFS "-D_VARIADIC_MAX=10") # rename variable
endif ()
#-------------------
# set common include folder for module
#-------------------
set(COMMON_INCLUDES ${PROJECT_SOURCE_DIR}/include) #sets files
set(EXT_PROJECTS_DIR ${PROJECT_SOURCE_DIR}/ext)

#-------------------
# Module source
#-------------------
#set(PROJECT_LIB_NAME ${PROJECT_NAME_STR}_impl)

#include_directories(${COMMON_INCLUDES})
#file(GLOB SRC_FILES ${PROJECT_SOURCE_DIR}/src/*.cpp)
#add_library(${PROJECT_LIB_NAME} ${SRC_FILES})

add_subdirectory(${EXT_PROJECTS_DIR}/gtest)

#-------------------
# Test
#-------------------
add_definitions(${MSVC_COMPILER_DEFS})
enable_testing() # enables testing? for this directories and subdirectories
set(PROJECT_TEST_NAME ${PROJECT_NAME_STR}_test) #new project test tname
include_directories(${GTEST_INCLUDE_DIRS} ${COMMON_INCLUDES}) #include directores given to the compiler

file(GLOB TEST_SRC_FILES ${PROJECT_SOURCE_DIR}/test/*.cpp)    #stores all files as test_src from dired
add_executable(${PROJECT_TEST_NAME} ${TEST_SRC_FILES})       #get built in the build tree directory as the NAME  from SRC FILES
add_dependencies(${PROJECT_TEST_NAME} googletest)           #dependenc typs inclue add_excecutable, add_library, or add_cstuomt taraget


if ( NOT WIN32 OR MINGW ) # not sure could be issue
    target_link_libraries(${PROJECT_TEST_NAME} # Project_Test_Name is the targett  # GTTest Libraries are the linked this is
            ${GTEST_LIBS_DIR}/libgtest.a  #might be problematic GTEST_LIBS looks undefined
            ${GTEST_LIBS_DIR}/libgtest_main.a
            )
else ()
    target_link_libraries(${PROJECT_TEST_NAME}
            debug ${GTEST_LIBS_DIR}/DebugLibs/${CMAKE_FIND_LIBRARY_PREFIXES}gtest${CMAKE_FIND_LIBRARY_SUFFIXES}
            optimized ${GTEST_LIBS_DIR}/ReleaseLibs/${CMAKE_FIND_LIBRARY_PREFIXES}gtest${CMAKE_FIND_LIBRARY_SUFFIXES}
            )
    target_link_libraries(${PROJECT_TEST_NAME}
            debug ${GTEST_LIBS_DIR}/DebugLibs/${CMAKE_FIND_LIBRARY_PREFIXES}gtest_main${CMAKE_FIND_LIBRARY_SUFFIXES}
            optimized ${GTEST_LIBS_DIR}/ReleaseLibs/${CMAKE_FIND_LIBRARY_PREFIXES}gtest_main${CMAKE_FIND_LIBRARY_SUFFIXES}
            )
endif ()

target_link_libraries(${PROJECT_TEST_NAME} ${CMAKE_THREAD_LIBS_INIT}) #Has to do with prethreeds

add_test(test1 ${PROJECT_TEST_NAME}) #add test by ctest by default slient
