# MonoKickstart
This is MonoKickstart, a standalone Mono "kick" application to run C# programs
on GNU/Linux and macOS without depending on a system installation of Mono.

## License
kick.c is released under the zlib license. See LICENSE for details.

`binreloc` is released under the WTFPL license. See `binreloc.LICENSE` for details.

-------------------
## About MonoKickstart

Originally developed by Edward Rudd for Bastion, MonoKickstart is a reworking of
the stock generated kickstart code from Mono to easily run C# applications on
*nix platforms. macOS support was added in 2013 for MonoGame-SDL2 titles.

## About the `precompiled/` Folder

We have provided a precompiled MonoKickstart application for you to use in your
programs. Included are `kick` binaries and `libmono` binaries for macOS and Linux
(both `x86` and `x86_64`, macOS uses universal binaries). We have also provided the
C# assemblies needed by MonoGame as well as a `./Kick` script to automatically
choose the right binary for the operating system and architecture.

To use the precompiled MonoKickstart, simply rename the `kick.bin.*` binaries to
the name of your `.exe` assembly (for instance, `MyGame.bin.x86` starts `MyGame.exe`).
Additionally, modify `Kick` to run those newly named `kick` binaries rather than
the generic "kick" name. The name of `Kick` itself can be whatever you like.

If you wish to use additional shared libraries (for instance, `libSDL2`), simply
add the library to the architecture's `lib` folder:

Linux (32-bit): `lib/`
Linux (64-bit): `lib64/`
macOS (Universal): `osx/`

### Building for macOS

For macOS we need Universal binaries. By default the `make` command from SDL will only build 64 bit.
The following commands should be used to generate a universal binary:

'''
  ./configure --disable-dependency-tracking
  make CXXFLAGS="-arch i386 -arch x86_64" CFLAGS="-arch i386 -arch x86_64" LDFLAGS="-arch i386 -arch x86_64"
'''
