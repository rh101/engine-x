
set(lib_name zlib)
set(target_name ext_${lib_name})

project(${lib_name})

include(../cmake/CocosExternalConfig.cmake)

if(WINDOWS)
  add_library(${target_name} SHARED IMPORTED GLOBAL)
else()
  add_library(${target_name} STATIC IMPORTED GLOBAL)
endif()

set(ZLIB_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/include" CACHE STRING "zlib include dir" FORCE)

if(MACOSX OR ANDROID)
  set(ZLIB_LIBRARY "${platform_spec_path}/libz.a" CACHE STRING "zlib library" FORCE)
  set_target_properties(${target_name} PROPERTIES
    IMPORTED_LOCATION "${platform_spec_path}/libz.a"
  )
elseif(WINDOWS)
  set(ZLIB_LIBRARY "${platform_spec_path}/libzlib.lib" CACHE STRING "zlib library" FORCE)
  set_target_properties(${target_name} PROPERTIES
    IMPORTED_LOCATION "${platform_spec_path}/zlib1.dll"
    IMPORTED_IMPLIB "${platform_spec_path}/libzlib.lib"
  )
endif()

set_target_properties(${target_name} PROPERTIES
    INTERFACE_INCLUDE_DIRECTORIES "${CMAKE_CURRENT_SOURCE_DIR}/include"
)
