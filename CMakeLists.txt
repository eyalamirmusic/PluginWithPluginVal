cmake_minimum_required(VERSION 3.16)
project(JUCECMakeRepo)

include(CMake/CPM.cmake)

if (${CMAKE_SYSTEM_NAME} STREQUAL "iOS")
    set(CMAKE_OSX_DEPLOYMENT_TARGET 11.0 CACHE STRING "Minimum OS X deployment version" FORCE)
    #code signing to run on an iOS device:
    #    set(CMAKE_XCODE_ATTRIBUTE_CODE_SIGN_IDENTITY "iPhone Developer" CACHE STRING "" FORCE)
    #    set(CMAKE_XCODE_ATTRIBUTE_DEVELOPMENT_TEAM "XXXXXXXXXX" CACHE STRING "" FORCE)
else ()
    set(CMAKE_OSX_DEPLOYMENT_TARGET "10.11" CACHE STRING "Minimum OS X deployment version" FORCE)
endif ()

option(UniversalBinary "Build universal binary for mac" OFF)

if (UniversalBinary)
    set(CMAKE_OSX_ARCHITECTURES "x86_64;arm64" CACHE INTERNAL "")
endif ()

set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")

set_property(GLOBAL PROPERTY USE_FOLDERS YES)
option(JUCE_ENABLE_MODULE_SOURCE_GROUPS "Enable Module Source Groups" ON)

CPMAddPackage("gh:juce-framework/JUCE#develop")

option(EA_USE_PLUGINVAL "" ON)

if (EA_USE_PLUGINVAL)
    CPMAddPackage(
            NAME pluginval
            GIT_REPOSITORY git@github.com:Tracktion/Pluginval.git
            GIT_TAG develop
            OPTIONS "PLUGINVAL_FETCH_JUCE OFF")
endif ()

add_subdirectory(Modules)
add_subdirectory(Plugins)