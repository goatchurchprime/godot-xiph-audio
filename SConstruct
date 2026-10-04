#!/usr/bin/env python

import os

from SCons.Script import ARGUMENTS, Default, Glob, SConscript

from tools.scons_helpers import build_cmake_dependency, library_path, output_library


# Keep godot-cpp small and deterministic unless a caller explicitly selects a
# different API profile.
ARGUMENTS.setdefault("build_profile", "build_profile.json")
env = SConscript("godot-cpp/SConstruct")
env = env.Clone()

# Never place generated objects beside source files (some of those sources are
# in submodules). This also keeps source archives and git status clean.
env["SHOBJPREFIX"] = "#obj/"
if os.environ.get("SCONS_CACHE"):
    env.CacheDir(os.environ["SCONS_CACHE"])
    env.Decider("MD5")

output_dir = ARGUMENTS.get("addon_output_dir", "addons/xiph_audio/bin")

env.Append(
    CPPPATH=[
        "src",
        "thirdparty/flac/include",
    ],
    # FLAC__NO_DLL prevents Windows headers from marking libFLAC symbols as
    # dllimport when we link the codec statically into the GDExtension.
    CPPDEFINES=["OP_HAVE_LRINTF", "FLAC__NO_DLL"],
    LIBPATH=[
        library_path(env, "thirdparty/flac", "src/libFLAC"),
    ],
    LIBS=["FLAC"],
)

if env["platform"] != "windows":
    env.Append(LIBS=["m"])

sources = Glob("src/*.cpp")

library = env.SharedLibrary(
    target=output_library(env, output_dir),
    source=sources,
)
env.NoCache(library)
Default(library)


def build_flac(target, source, env):
    options = [
        "-DBUILD_SHARED_LIBS=OFF",
        "-DBUILD_CXXLIBS=OFF",
        "-DBUILD_PROGRAMS=OFF",
        "-DBUILD_EXAMPLES=OFF",
        "-DBUILD_TESTING=OFF",
        "-DBUILD_DOCS=OFF",
        "-DBUILD_UTILS=OFF",
        "-DINSTALL_MANPAGES=OFF",
        "-DINSTALL_PKGCONFIG_MODULES=OFF",
        "-DINSTALL_CMAKE_CONFIG_MODULE=OFF",
        "-DWITH_OGG=OFF",
        "-DENABLE_MULTITHREADING=OFF",
    ]
    # libFLAC 1.5.0 misdetects fseeko on 32-bit Android even though Godot's
    # NDK target is API 24, then aliases the available function to fseek and
    # creates conflicting declarations in the NDK headers.
    if env["platform"] == "android" and env["arch"] in ("arm32", "x86_32"):
        options.append("-DCMAKE_C_FLAGS=-DHAVE_FSEEKO=1")
    if env["platform"] == "windows" and env.get("use_static_cpp", True) and not env.get("debug_crt", False):
        options.append("-DCMAKE_MSVC_RUNTIME_LIBRARY=MultiThreaded")
    return build_cmake_dependency(env, "thirdparty/flac", options)


env.Command("build_flac", [], build_flac)
