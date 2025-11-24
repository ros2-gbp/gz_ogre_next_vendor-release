cmake_minimum_required(VERSION 3.10)
project(gz_ogre_next_vendor)

# Project-specific settings
set(LIB_VER_MAJOR 2)
set(LIB_VER_MINOR 3)
set(LIB_VER_PATCH 3)

# Derived variables
set(LIB_VER ${LIB_VER_MAJOR}.${LIB_VER_MINOR}.${LIB_VER_PATCH})

find_package(ament_cmake REQUIRED)
find_package(ament_cmake_vendor_package REQUIRED)

# Required for FindGzOGRE2.cmake and gz_find_package
find_package(gz_cmake_vendor REQUIRED)
find_package(gz-cmake REQUIRED)

# Set the VERSION_MATCH to "EXACT" by default, but relax the requirement
# if we are users are building from source (determined by the
# GZ_BUILD_FROM_SOURCE environment variable) or if explicitly told to do so
# by the GZ_RELAX_VERSION_MATCH environment variable.
set(VERSION_MATCH "EXACT")
if(NOT $ENV{GZ_BUILD_FROM_SOURCE} STREQUAL "")
  set(VERSION_MATCH "")
endif()

if(NOT $ENV{GZ_RELAX_VERSION_MATCH} STREQUAL "")
  set(VERSION_MATCH "")
endif()

gz_find_package(GzOGRE2
  EXACT ${VERSION_MATCH}
  VERSION ${LIB_VER}
  COMPONENTS
  HlmsPbs
  HlmsUnlit
  Overlay
  PlanarReflections
  PRIVATE_FOR ogre2
  QUIET
)

set(EXTRA_CMAKE_FLAGS)
if(CMAKE_SYSTEM_PROCESSOR MATCHES "^(aarch64.*|AARCH64.*|arm64.*|ARM64.*|arm.*|ARM.*)")
  # Flags for non SIMD architectures
  # https://github.com/gazebo-forks/ogre-2.3-release/blob/960bb19664879282b979e4ed8cb3ce278c875bdb/debian/rules#L21-L28
  set(EXTRA_CMAKE_FLAGS -DOGRE_SIMD_NEON:BOOL=FALSE  -DOGRE_SIMD_SSE2:BOOL=FALSE)
endif()

ament_vendor(${PROJECT_NAME}
SATISFIED ${OGRE2_FOUND}
VCS_URL https://github.com/OGRECave/ogre-next.git
  VCS_VERSION v${LIB_VER}
  CMAKE_ARGS
    -DOGRE_STATIC:BOOL=OFF
    -DOGRE_BUILD_COMPONENT_OVERLAY:BOOL=ON
    -DOGRE_BUILD_COMPONENT_PLANAR_REFLECTIONS:BOOL=TRUE
    -DOGRE_BUILD_PLUGIN_PFX:BOOL=ON
    -DOGRE_BUILD_RENDERSYSTEM_GL3PLUS:BOOL=TRUE
    -DOGRE_BUILD_RENDERSYSTEM_GLES2:BOOL=FALSE
    -DOGRE_BUILD_RENDERSYSTEM_VULKAN:BOOL=TRUE
    -DOGRE_BUILD_SAMPLES2:BOOL=FALSE
    -DOGRE_CONFIG_UNIX_NO_X11:BOOL=FALSE
    -DOGRE_FULL_RPATH:BOOL=FALSE
    -DOGRE_GLSUPPORT_USE_EGL_HEADLESS:BOOL=TRUE
    -DOGRE_GLSUPPORT_USE_GLX:BOOL=TRUE
    -DOGRE_INSTALL_SAMPLES:BOOL=FALSE
    -DOGRE_USE_NEW_PROJECT_NAME:BOOL=ON
    -DOGRE_VULKAN_WINDOW_NULL:BOOL=TRUE
    -DOGRE_VULKAN_WINDOW_XCB:BOOL=ON
    ${EXTRA_CMAKE_FLAGS}
  GLOBAL_HOOK
  PATCHES
    patches/0001-Fix-incomplete-vulkan-linkage.patch
)

message("OGRE2_FOUND: ${OGRE2_FOUND}")
message("OGRE2_VERSION: ${OGRE2_VERSION}")
message("OGRE2_INCLUDE_DIRS: ${OGRE2_INCLUDE_DIRS}")
message("OGRE2_LIBRARIES: ${OGRE2_LIBRARIES}")

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package(
  CONFIG_EXTRAS_POST "gz_ogre_next_vendor-extras.cmake.in"
)
