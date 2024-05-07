cmake_minimum_required(VERSION 3.10)
project(gz_ogre_next_vendor)

find_package(ament_cmake REQUIRED)
find_package(ament_cmake_vendor_package REQUIRED)

set(EXTRA_CMAKE_FLAGS)
if(CMAKE_SYSTEM_PROCESSOR MATCHES "^(aarch64.*|AARCH64.*|arm64.*|ARM64.*|arm.*|ARM.*)")
  # Flags for non SIMD architectures
  # https://github.com/gazebo-forks/ogre-2.3-release/blob/960bb19664879282b979e4ed8cb3ce278c875bdb/debian/rules#L21-L28
  set(EXTRA_CMAKE_FLAGS -DOGRE_SIMD_NEON:BOOL=FALSE  -DOGRE_SIMD_SSE2:BOOL=FALSE)
endif()

ament_vendor(${PROJECT_NAME}
  VCS_URL https://github.com/OGRECave/ogre-next.git
  VCS_VERSION v2.3.3
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

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package(
  CONFIG_EXTRAS_POST "gz_ogre_next_vendor-extras.cmake.in"
)
