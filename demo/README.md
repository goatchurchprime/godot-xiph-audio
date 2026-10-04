# Interactive FLAC and Opus demo

Run this Godot project to compare seek, playback, and looping between FLAC
decoded by this addon, Opus decoded by Joyless's
[Opus GDExtension](https://github.com/Joy-less/OpusGdextension), and Godot's
native Ogg Vorbis and MP3 streams. The two third-party addons remain separate;
the live demo installs both so their stream types can be exercised together.

To download a focused FLAC conformance set covering 96 kHz/24-bit, 5.1,
unknown total length, and mono streams:

```bash
bash tools/fetch_flac_test_files.sh
```

Downloaded fixtures appear in the picker automatically. They come from the
[IETF CELLAR FLAC decoder testbench](https://github.com/ietf-wg-cellar/flac-test-files)
and are dedicated to the public domain under CC0 1.0. Multichannel samples are
useful compatibility tests; this addon currently presents the first two FLAC
channels through Godot's stereo `AudioFrame` playback interface rather than
performing a surround-aware downmix.

## Web export

The included `Web` export preset produces a single-threaded build with
GDExtension support. Install Godot 4.6 Web export templates, copy this packaged
addon into `demo/addons/xiph_audio`, and copy Opus GDExtension into
`demo/addons/OpusGdextension`. Then run:

```bash
godot --headless --path demo --export-release Web ../dist/web/index.html
```

For itch.io, upload the contents of `dist/web` as an HTML5 build. The entry
point is `index.html`. The public Opus addon is also available from the
[Godot Asset Store](https://store.godotengine.org/asset/joyless/opus-gdextension/).
