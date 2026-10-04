# FLAC support for Godot

This GDExtension loads FLAC (`.flac`) files as native `AudioStreamFLAC`
resources. Use them with `load()`, preload them in scenes, or assign them to an
`AudioStreamPlayer` like built-in audio formats.

This addon no longer includes Ogg Opus support. Use
[Opus GDExtension](https://github.com/Joy-less/OpusGdextension) for `.opus`
files.

## Install

Copy this `xiph_audio` directory into your project's `addons` directory. The
extension loads automatically; there is no EditorPlugin to enable.

The archive includes native libraries for Linux and Windows x86-64, universal
macOS, iOS arm64, Android arm32/arm64/x86-32/x86-64, and threaded and
single-threaded Web exports. Godot 4.5 or newer is required.

Source, documentation, and issue tracker:
https://github.com/goatchurchprime/godot-xiph-audio
