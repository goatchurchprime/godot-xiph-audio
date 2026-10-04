cmake_minimum_required(VERSION 3.20)
project(godot_xiph_audio VERSION 0.3.0 LANGUAGES C CXX)

set(BUILD_SHARED_LIBS OFF CACHE BOOL "" FORCE)
set(BUILD_CXXLIBS OFF CACHE BOOL "" FORCE)
set(BUILD_PROGRAMS OFF CACHE BOOL "" FORCE)
set(BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
set(BUILD_TESTING OFF CACHE BOOL "" FORCE)
set(BUILD_DOCS OFF CACHE BOOL "" FORCE)
set(BUILD_UTILS OFF CACHE BOOL "" FORCE)
set(INSTALL_MANPAGES OFF CACHE BOOL "" FORCE)
set(INSTALL_PKGCONFIG_MODULES OFF CACHE BOOL "" FORCE)
set(INSTALL_CMAKE_CONFIG_MODULE OFF CACHE BOOL "" FORCE)
set(WITH_OGG OFF CACHE BOOL "" FORCE)

set(GODOTCPP_BUILD_PROFILE "${CMAKE_CURRENT_SOURCE_DIR}/build_profile.json" CACHE FILEPATH "" FORCE)
add_subdirectory(godot-cpp)
add_subdirectory(thirdparty/flac EXCLUDE_FROM_ALL)

# These static archives are linked into the GDExtension shared library.
# Some toolchains enable PIC implicitly; make the requirement explicit so
# clean distribution builds do not depend on that toolchain behaviour.
set_target_properties(FLAC PROPERTIES POSITION_INDEPENDENT_CODE ON)

add_library(godot_xiph_audio SHARED
    src/audio_stream_flac.cpp
    src/resource_format_loader_flac.cpp
    src/register_types.cpp
)
target_include_directories(godot_xiph_audio PRIVATE src thirdparty/flac/include)
target_link_libraries(godot_xiph_audio PRIVATE godot-cpp FLAC)
target_compile_features(godot_xiph_audio PRIVATE cxx_std_17)
set_target_properties(godot_xiph_audio PROPERTIES
    CXX_EXTENSIONS OFF
    CXX_VISIBILITY_PRESET hidden
    POSITION_INDEPENDENT_CODE ON
    BUILD_RPATH_USE_ORIGIN ON
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/addons/xiph_audio/bin"
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/addons/xiph_audio/bin"
)
get_target_property(GODOTCPP_SUFFIX godot-cpp GODOTCPP_SUFFIX)
set_target_properties(godot_xiph_audio PROPERTIES
    PREFIX "lib"
    OUTPUT_NAME "xiph_audio${GODOTCPP_SUFFIX}"
)
