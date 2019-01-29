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
  set(host_name "windows")
elseif(CMAKE_HOST_APPLE)
  set(host_name "darwin")
elseif(CMAKE_HOST_UNIX)
  # Assuming Linux
  set(host_name "linux")
else()
  message(FATAL_ERROR "Unexpected host")
endif()

foreach(arch armel x86_64 mips64el mipsel aarch64 i386)
  install(
      FILES
      "${ANDROID-SDK-TOOLS_ROOT}/qemu/${host_name}-x86_64/qemu-system-${arch}${CMAKE_EXECUTABLE_SUFFIX}"
      DESTINATION "android-sdk/tools/qemu/${host_name}-x86_64/"
      PERMISSIONS
          OWNER_WRITE
          OWNER_READ GROUP_READ WORLD_READ
          OWNER_EXECUTE GROUP_EXECUTE WORLD_EXECUTE
  )
endforeach()

foreach(tool e2fsck resize2fs tune2fs)
  install(
      FILES
      "${ANDROID-SDK-TOOLS_ROOT}/bin64/${tool}${CMAKE_EXECUTABLE_SUFFIX}"
      DESTINATION "android-sdk/tools/bin64/"
      PERMISSIONS
          OWNER_WRITE
          OWNER_READ GROUP_READ WORLD_READ
          OWNER_EXECUTE GROUP_EXECUTE WORLD_EXECUTE
  )
endforeach()
