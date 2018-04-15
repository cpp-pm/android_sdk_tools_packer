# Copyright (c) 2018, Ruslan Baratov
# All rights reserved.

cmake_minimum_required(VERSION 3.3)

include("cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/ruslo/hunter/archive/v0.20.40.tar.gz"
    SHA1 "a500979296b4b6b1542a2881151ceb9c984f3e36"
)

project(android_sdk_tools_packer)

hunter_add_package(android_sdk_packer)

# android_sdk_packer_unified_install
find_package(android_sdk_packer CONFIG REQUIRED)

hunter_add_package(Android-SDK-Tools)
file(GLOB x "${ANDROID-SDK-TOOLS_ROOT}/*")
android_sdk_packer_unified_install(OBJECTS ${x} DESTINATION "tools")
