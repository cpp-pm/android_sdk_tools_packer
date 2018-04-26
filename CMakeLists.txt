# Copyright (c) 2018, Ruslan Baratov
# All rights reserved.

cmake_minimum_required(VERSION 3.3)

include("cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/ruslo/hunter/archive/v0.20.57.tar.gz"
    SHA1 "4f4608e1c11b67df417bfc5a68d4b5a86251b852"
)

project(android_sdk_tools_packer)

hunter_add_package(android_sdk_packer)

# android_sdk_packer_unified_install
find_package(android_sdk_packer CONFIG REQUIRED)

hunter_add_package(Android-SDK-Tools)
file(GLOB x "${ANDROID-SDK-TOOLS_ROOT}/*")
android_sdk_packer_unified_install(OBJECTS ${x} DESTINATION "tools")

if(CMAKE_HOST_WIN32)
  # TODO
elseif(CMAKE_HOST_APPLE)
  foreach(arch armel x86_64 mips64el mipsel aarch64 i386)
    install(
        FILES "${ANDROID-SDK-TOOLS_ROOT}/qemu/darwin-x86_64/qemu-system-${arch}"
        DESTINATION "android-sdk/tools/qemu/darwin-x86_64/"
        PERMISSIONS
            OWNER_WRITE
            OWNER_READ GROUP_READ WORLD_READ
            OWNER_EXECUTE GROUP_EXECUTE WORLD_EXECUTE
    )
  endforeach()
elseif(CMAKE_HOST_UNIX)
  # Assuming Linux

  foreach(arch armel x86_64 mips64el mipsel aarch64 i386)
    install(
        FILES "${ANDROID-SDK-TOOLS_ROOT}/qemu/linux-x86_64/qemu-system-${arch}"
        DESTINATION "android-sdk/tools/qemu/linux-x86_64/"
        PERMISSIONS
            OWNER_WRITE
            OWNER_READ GROUP_READ WORLD_READ
            OWNER_EXECUTE GROUP_EXECUTE WORLD_EXECUTE
    )
  endforeach()
else()
  message(FATAL_ERROR "Unexpected host")
endif()
