# FLAC support for Godot

A cross-platform GDExtension that loads FLAC (`.flac`) files as native Godot
`AudioStreamFLAC` resources.

```gdscript
var music := load("res://music.flac") as AudioStreamFLAC
$AudioStreamPlayer.stream = music
$AudioStreamPlayer.play()
```

Streams support duration reporting, playback, seeking, simultaneous playbacks,
and optional looping with a loop offset. Compressed bytes remain in the
resource and are decoded incrementally instead of being expanded into a
complete WAV buffer.

## Opus support

This addon no longer provides Ogg Opus playback. For `.opus` files, use the
human-maintained [Opus GDExtension](https://github.com/Joy-less/OpusGdextension),
available from the
[Godot Asset Store](https://store.godotengine.org/asset/joyless/opus-gdextension/).
It provides `AudioStreamOpus` with dedicated editor importing, metadata, and
preview support.

The previous `AudioStreamOggOpus` implementation remains in the `v0.2.0`
release for historical reference, but new projects should use Opus GDExtension.

## Install

Download the release archive and extract it at the root of a Godot project. It
contains only `addons/xiph_audio`. The GDExtension loads automatically; there
is no EditorPlugin to enable.

Release archives support Linux and Windows x86-64, universal macOS, iOS arm64,
all four Android architectures, and threaded or single-threaded Web exports.
The minimum supported engine version is Godot 4.5.

## Build and test on NixOS

```bash
git submodule update --init --recursive
bash tools/build_linux.sh
bash tools/smoke_linux.sh
```

Distribution builds use SCons:

```bash
python -m pip install scons
scons build_flac platform=linux target=template_debug arch=x86_64
scons platform=linux target=template_debug arch=x86_64
```

libFLAC is statically linked into the GDExtension binary.

## Bundled dependency versions

Release builds use the Git submodule commits pinned by this repository. They do
not automatically follow upstream branches, which keeps rebuilds reproducible.

| Dependency | Version used | Pinned source | Release Date |
| --- | --- | --- | --- |
| [libFLAC](https://github.com/xiph/flac) | 1.5.0 | [`1507800d`](https://github.com/xiph/flac/commit/1507800de4b70e21be71f38caa0d9079d0bc6e45) | 11 February 2025 |
| [godot-cpp](https://github.com/godotengine/godot-cpp) | Godot 4.5 branch checkout | [`27d9dd23`](https://github.com/godotengine/godot-cpp/commit/27d9dd23c83871e0619fca5dc2cddfbfd69e926a) | 25 August 2026 |

Updating either submodule is an explicit maintenance change followed by the
complete platform build matrix.

## Interactive demo and conformance fixtures

The `demo` project provides shared play/pause, seek, position, and loop controls
for FLAC and Godot's native WAV, Ogg Vorbis, and MP3 streams. See
`demo/README.md` for the focused CC0 FLAC conformance set.

## Releases and Godot Asset Store

Each GitHub Actions build produces `godot-xiph-audio.zip`. Tags publish the
same archive to a GitHub release. Use that release archive—not GitHub's source
archive—because source archives omit compiled binaries and submodule contents.

The current Godot Asset Store entry is
[Xiph Audio Streams](https://store.godotengine.org/asset/goatchurch/xiph-audio-streams/).
Its next version will be FLAC-only. The 16:9 listing thumbnail is
`docs/asset-store-thumbnail.png`.

Third-party sources retain their licenses: libFLAC (BSD-style) and godot-cpp
(MIT). See the packaged notices for details.

## AI assistance

OpenAI Codex was used as a coding agent to discuss scope, implement substantial
portions of the GDExtension and FLAC support, add cross-platform build and
release packaging, and diagnose build failures. Julian Todd directed and
reviewed the work and remains responsible for the project and its releases.
