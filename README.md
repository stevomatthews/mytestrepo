# mytestrepo
Repo to be used for learning git, GitHub, and GitHub Desktop (Mac)

The [table below](#list-of-options) provides a complete list of SRT options and 
their characteristics according to the following legend:

1. **Since**: Defines the SRT version when this option was first introduced. If this field
is empty, it's an option derived from UDT. "Version 0.0.0" is the oldest version
of SRT ever created and put into use.

2. **Binding**: Defines limitation on setting the option. The field is empty if the option
is not settable (see **Dir** column):

    - `pre`: A connecting socket (both as caller and rendezvous) must be set
prior to calling `srt_connect()` or `srt_bind()` and never changed thereafter.
A listener socket should be set to "listening" and it will be
derived by every socket returned by `srt_accept()`.

    - `pre-bind`: The option cannot be altered on a socket that is already bound (by calling `srt_bind()` or any other function doing this, including automatic binding when trying to connect, as well as accepted sockets). The `pre-bind` characteristic applies exclusively to options that:
      - change the behavior and functionality of the `srt_bind` call
      - concern or set an option on the internally used UDP socket
      - concern any kind of resource used by the multiplexer


| Library       | Version   | Description / License Information                   |
|:------------- | :-------- | :-------------------------------------------------- |
| Aften         | 0.0.8     | Audio Encoder Library: http://aften.sourceforge.net/ <br>License: [GNU Library General Public License (LGPL) v2.1](https://www.gnu.org/licenses/lgpl-2.1.html) |
| AudioFile     | 0.3.6     | AudioFile Library: https://github.com/mpruett/audiofile/ <br>License: [GNU Library General Public License (LGPL) v2.1](https://www.gnu.org/licenses/lgpl-2.1.html) |
| Azure-iot-sdk | 20190318  | Azure IoT SDK:  https://github.com/Azure/azure-iot-sdk-c <br>License: MIT-Style https://github.com/Azure/azure-iot-sdk-c/blob/master/LICENSE |
| BOOST         | 1_78_0<br>1_75_0<br>1_73_0 | BOOST Portable C++ Libraries: http://www.boost.org/ <br>License: [BOOST Software License v1.0](https://www.boost.org/LICENSE_1_0.txt) |
| BOOST-process | 0.5       | BOOST.Process Library: http://www.highscore.de/boost/process0.5/index.html <br>License: [BOOST Software License v1.0](https://www.boost.org/LICENSE_1_0.txt) |
| BZip2         | 1.0.8     | BZip2 Compression Library: http://www.sourceware.org/bzip2/ <br>License: [Berkeley Software Distribution (BSD) Style License](https://en.wikipedia.org/wiki/BSD_licenses) |
| CMRT          | 1.0.6     | Media GPU kernel manager for Intel G45 & HD Graphics: https://github.com/intel/cmrt <br>License: MIT-Style https://github.com/intel/cmrt/blob/master/LICENSE |
| DesktopVideo  | 12.0.0<br>11.6.0 | BlackMagic Design Desktop Video Utilities, Libraries, Firmware, and Drivers: http://blackmagicdesign.com <br>License: Proprietary |
| expat         | 2.4.4     | XML Parser Library: http://www.libexpat.org/ <br>License: MIT-Style https://github.com/libexpat/libexpat/blob/master/expat/COPYING |
| ElfUtils      | 0.170     | Utilities/Libraries to process ElfFiles: https://sourceware.org/elfutils/ <br>License: [GNU Library General Public License (LGPL) v3.0](https://www.gnu.org/licenses/lgpl-3.0.html) |
| FAAC          | 1.30      | Freeware AAC Codec: https://sourceforge.net/projects/faac/ <br>License: [GNU Library General Public License (LGPL) v2.0](https://www.gnu.org/licenses/lgpl-2.0.html) |
| fdk-aac       | 2.0.1     | Fraunhofer OpenSource AAC Codec: http://opencore-amr.sourceforge.net/ <br>License: [Apache Sofware License (ASL) v2.0](https://www.apache.org/licenses/LICENSE-2.0) |
| FFMPEG        | 5.1-DEV   | Cross platform solution to record, convert, and stream audio and video: https://www.ffmpeg.org/ <br>License: [GNU Library General Public License (LGPL) v2.1](https://www.gnu.org/licenses/lgpl-2.1.html) |
| FFNVCodec     | 11.0.10.0<br>9.1.23.2 | NVidia Codec Headers for use with FFMPEG: https://github.com/FFmpeg/nv-codec-headers <br>License: BSD-Style [Berkeley Software Distribution (BSD) Style License](https://en.wikipedia.org/wiki/BSD_licenses) |
| FLAC          | 1.3.3     | Free Lossless Audio Codec: http://flac.sourceforge.net/ <br>License: BSD-Style https://github.com/webmproject/libvpx/blob/master/LICENSE |
| FLTK          | 1.4.0     | Fast Light Tool Kit (FLTK) Graphical User Interface Toolkit: http://www.fltk.org/ <br>License: GNU Library General Public License with an exception that allows statically-linked programs using the library without providing source to the program or library. [FLTK Library License](https://www.fltk.org/doc-1.4/license.html) |
| FreeType2     | 2.10.4    | Free and portable font rendering engine: http://www.freetype.org <br>License: Free-Type BSD-Style https://www.freetype.org/license.html |
| FreeBSD       | 12.1      | Imported Components from the FreeBSD Runtime. In particular `inet_*.c` functions: https://github.com/freebsd <br>License: BSD-Style [Berkeley Software Distribution (BSD) Style License](https://en.wikipedia.org/wiki/BSD_licenses) |
| GLAD          | 0.1.33    | Multi-Language Vulkan/GL/GLES/EGL/GLX/WGL Loader-Generator: https://github.com/Dav1dde/glad <br>License: [Massachusetts Institute of Technology (MIT) License](https://en.wikipedia.org/wiki/MIT_License) |
| GLEW          | 2.2.0     | OpenGL Extension Wrangler Library: https://sourceforge.net/projects/glew <br> License: [Modified BSD-Style, Mesa3D (MIT) License and Khoros Group (MIT) License](https://github.com/nigels-com/glew/blob/master/LICENSE.txt) |
| GLFW          | 3.3.2     | Open Source, multi-platform library for OpenGL, OpenGL ES and Vulkan develoment: https://www.glfw.org/ <br> License: [ZLIB Library](https://www.zlib.net/zlib_license.html) |
| GLM           | 0.9.9.8   | OpenGL Mathematics Library: https://glm.g-truc.net/ <br> License: [Modified MIT License](https://github.com/g-truc/glm/blob/master/copying.txt) |
| GLIB2         | 2.55.0    | Library of handy utility functions: http://www.gtk.org <br> License: [GNU Library General Public License (LGPL) v2.0](https://www.gnu.org/licenses/lgpl-2.0.html) |
| iconv         | 1.16      | Unicode and user/system string conversion library: https://www.gnu.org/software/libiconv/ <br>License: [GNU Library General Public License (LGPL) v2.0](https://www.gnu.org/licenses/lgpl-2.0.html) |
| isc-bindutils | 9.11.0-P1 | ISC NS Software Components: http://www.isc.org/downloads/ <br>License: BSD-Style [Berkeley Software Distribution (BSD) Style License](https://en.wikipedia.org/wiki/BSD_licenses) |
| gmmlib        | 20.3.2    | Intel(R) Graphics Memory Management Library: https://github.com/intel/gmmlib <br>License: [Massachusetts Institute of Technology (MIT) License](https://en.wikipedia.org/wiki/MIT_License) |
| IQA           | 1.1.2     | Image Quality Assessment: http://tdistler.com/iqa <br>License: BSD-Style [Berkeley Software Distribution (BSD) Style License](https://en.wikipedia.org/wiki/BSD_licenses) and [Massachusetts Institute of Technology (MIT) License](https://en.wikipedia.org/wiki/MIT_License) |
| LAME          | 3.100     | MPEG1 Layer III Audio Encoder: http://lame.sourceforge.net/ <br>License: [GNU Library General Public License (LGPL) v2.0](https://www.gnu.org/licenses/lgpl-2.0.html) |
| libao         | 1.2.0     | Cross platform audio library: http://www.xiph.org/ao/ <br>License: BSD-Style [Berkeley Software Distribution (BSD) Style License](https://en.wikipedia.org/wiki/BSD_licenses) |
| libcurl       | 7.73.0<br>7.44.0 | Multiprotocol File Transfer Library: https://curl.haxx.se/libcurl/ <br>License: MIT-Style https://curl.haxx.se/docs/faq.html#What_are_my_obligations_when_usi |
| libdrm        | 2.4.100   | Direct Rendering Manager runtime library: http://dri.sourceforge.net <br>License: [Massachusetts Institute of Technology (MIT) License](https://en.wikipedia.org/wiki/MIT_License) |
| libffi        | Various   | Portable, high level programming interface to various calling conventions https://sourceware.org/libffi/ <br>License: BSD-Style https://github.com/libffi/libffi/blob/master/LICENSE |
| libgcc        | Various   | GCC Runtime Library: http://gcc.gnu.org/onlinedocs/gccint/Libgcc.html <br>License: [GCC Runtime Library Exception](https://www.gnu.org/licenses/gcc-exception-3.1.en.html) and [GNU Linking Exception](https://en.wikipedia.org/wiki/GPL_linking_exception) |
| liblzma       | 5.2.5     | Compressoin Library: http://tukaani.org/xz/ <br>License: [Public Domain License](https://en.wikipedia.org/wiki/Public_domain) |
| libogg        | 1.3.4     | Ogg bitstream file format library: http://www.xiph.org/ <br>License: BSD-Style [Berkeley Software Distribution (BSD) Style License](https://en.wikipedia.org/wiki/BSD_licenses) |
| libpciaccess  | 0.16      | PCI access library: http://gitweb.freedesktop.org/?p=xorg/lib/libpciaccess.git <br>License: [Massachusetts Institute of Technology (MIT) License](https://en.wikipedia.org/wiki/MIT_License) |
| libpng        | 1.6.37    | Library for manipulating PNG images: http://www.libpng.org/pub/png/ <br>License: [ZLIB Library](https://www.zlib.net/zlib_license.html) |
| libpthread-stubs | 0.1    | Library that provides weak aliases for pthread functions not in the c runtime or not available by default: https://xcb.freedesktop.org/dist/ <br>License: [Massachusetts Institute of Technology (MIT) License](https://en.wikipedia.org/wiki/MIT_License) |
| librist       | 0.2.6     | Reliable Internet Stream Transport (RIST) Library: https://code.videolan.org/rist/librist <br>License: BSD-Style https://code.videolan.org/rist/rist-utils/-/blob/master/COPYING |
| libsamplerate | 0.1.9     | Sample rate conversion library: http://www.mega-nerd.com/SRC/ <br>License: BSD-Style http://www.mega-nerd.com/SRC/license.html |
| libsndfile    | 1.0.28    | Library for reading and writing sound files: http://www.mega-nerd.com/libsndfile/ <br>License: [GNU Library General Public License (LGPL) v2.0](https://www.gnu.org/licenses/lgpl-2.0.html) |
| libsodium     | 1.0.18    | Encryption, decryption, signature, and password hashing library with an API compatible with NaCl: https://github.com/jedisct1/libsodium/ <br>License: [Internet Systems Consortium (ISC) License](https://opensource.org/licenses/ISC) |
| libspng       | 0.7.0     | Simple PNG Library: https://libspng.org/ <br>License: BSD-Style [Berkeley Software Distribution (BSD) Style License](https://en.wikipedia.org/wiki/BSD_licenses) |
| libstdc++     | Various   | GNU C++ Runtime Library https://gcc.gnu.org/onlinedocs/libstdc++/ <br>License: [LIBSDTC++ Runtime Library Exception](https://gcc.gnu.org/onlinedocs/libstdc++/manual/license.html) and [LIBSDTC++ Runtime Library Exception FAQ](https://gcc.gnu.org/onlinedocs/libstdc++/faq.html#faq.license.what) |
| libudev       | Various   | Library to access udev device information: http://www.kernel.org/pub/linux/utils/kernel/hotplug/udev.html <br>License: [GNU Library General Public License (LGPL) v2.0](https://www.gnu.org/licenses/lgpl-2.0.html) |
| libusb        | Various   | Library which allows userspace access to USB devices: http://sourceforge.net/projects/libusb/ <br>License: [GNU Library General Public License (LGPL) v2.0](https://www.gnu.org/licenses/lgpl-2.0.html) |
| libuuid       | 2.36.1    | Universally unique ID library: ftp://ftp.kernel.org/pub/linux/utils/util-linux-ng <br>License: BSD-Style [Berkeley Software Distribution (BSD) Style License](https://en.wikipedia.org/wiki/BSD_licenses) |
| libvorbis     | 1.3.7     | Vorbis General Audio Compression Codec: http://www.xiph.org/ <br>License: BSD-Style [Berkeley Software Distribution (BSD) Style License](https://en.wikipedia.org/wiki/BSD_licenses) |
| libvpx        | Dev       | VP8/VP9 Codec and WebM library: https://www.webmproject.org/code/ <br>License: BSD-Style https://github.com/webmproject/libvpx/blob/master/LICENSE |
| libyuv        | v1768     | YUV scaling and conversion functionality: https://chromium.googlesource.com/libyuv/libyuv/ <br>License: BSD-Style https://chromium.googlesource.com/libyuv/libyuv/+/master/LICENSE |
| LMOParser     | 1.7       | Command line option parser: http://optionparser.sourceforge.net/index.html <br>License: [Massachusetts Institute of Technology (MIT) License](https://en.wikipedia.org/wiki/MIT_License) |
| msinttypes    | r29       | ISO C9x compliant standard inttypes: http://code.google.com/p/msinttypes/ <br>License: BSD-Style [Berkeley Software Distribution (BSD) Style License](https://en.wikipedia.org/wiki/BSD_licenses) |
| numactl       | 2.0.13    | Library for tuning for Non Uniform Memory Access: ftp://oss.sgi.com/www/projects/libnuma/download <br>License: [GNU Library General Public License (LGPL) v2.1](https://www.gnu.org/licenses/lgpl-2.1.html) |
| OpenBSD       | 6.6       | Imported Components from the OpenBSD Runtime. In particular `str*.c` functions: https://github.com/openbsd <br>License: BSD-Style [Berkeley Software Distribution (BSD) Style License](https://en.wikipedia.org/wiki/BSD_licenses) |
| OpenSRT       | 1.4.4<br>1.4.3 | Secure, reliable transport (SRT) protocol library: https://github.com/Haivision/srt <br>License: [Mozilla Public License (MPL)](https://www.mozilla.org/en-US/MPL/) |
| OpenSSL       | 1.1.1i<br>1.0.2t | Cryptography library with TLS implementation: http://www.openssl.org/ <br>License: https://www.openssl.org/source/license.html |
| PortAudio     | v190600   | Portable, realtime audio IO library: http://www.portaudio.com/download.html <br>License: http://www.portaudio.com/license.html |
| ProMPEG Decoder | Dev     | ProMPEG Decoder https://github.com/kirintwn/prompeg-decoder <br>License: MIT-Style https://github.com/kirintwn/prompeg-decoder/blob/master/LICENSE |
| ProtoBuf      | 2.4.1<br>2.5.0 | Google Protocol Buffers for Data Interchange Format library: https://github.com/google/protobuf <br>License: BSD-Style [Berkeley Software Distribution (BSD) Style License](https://en.wikipedia.org/wiki/BSD_licenses) |
| SDL2          | 2.0.12<br>2.0.10<br>2.0.4 | Simple Direct Media Layer v2: http://www.libsdl.org/index.php <br>License: https://wiki.libsdl.org/FAQLicensing |
| SOIL          | 1.16      | Simple OpenGL Image library: http://www.lonesock.net/soil.html <br>License: [Public Domain License](https://en.wikipedia.org/wiki/Public_domain) |
| SPEEX         | 1.2.rc1   | Voice compression format (codec): http://www.speex.org/ <br>License: BSD-Style [Berkeley Software Distribution (BSD) Style License](https://en.wikipedia.org/wiki/BSD_licenses) |
| TWOLAME       | 0.4.0     | MPEG1 Layer II audio encoder: http://www.twolame.org/ <br>License: [GNU Library General Public License (LGPL) v2.1](https://www.gnu.org/licenses/lgpl-2.1.html) |
| URIParser     | 0.9.4     | URI parser library: https://uriparser.github.io/ <br>License: BSD-Style [Berkeley Software Distribution (BSD) Style License](https://en.wikipedia.org/wiki/BSD_licenses) |
| UTF8-CPP      | 2.3.4     | Library for handling UTF-8 encoded strings: http://utfcpp.sourceforge.net/ <br>License: [Massachusetts Institute of Technology (MIT) License](https://en.wikipedia.org/wiki/MIT_License) |
| V4L2          | 1.8.1     | Video for Linux (V4L2) API: http://linuxtv.org/downloads/v4l-utils/ <br>License: [GNU Library General Public License (LGPL) v2.1](https://www.gnu.org/licenses/lgpl-2.1.html) |
| VAAPI         | 2.10.0    | Video Acceleration API: https://github.com/intel/libva/ https://github.com/intel/libva-utils/ https://github.com/intel/intel-vaapi-driver/ https://github.com/intel/media-driver/ <br>License: BSD-Style [Berkeley Software Distribution (BSD) Style License](https://en.wikipedia.org/wiki/BSD_licenses) and [Massachusetts Institute of Technology (MIT) License](https://en.wikipedia.org/wiki/MIT_License) |
| ZeroMQ        | 4.3.4<br>4.3.1 | Multi-platform distributed RPC library: http://zeromq.org/ <br>License: [GNU Library General Public License (LGPL) v3.0](https://www.gnu.org/licenses/lgpl-3.0.html) |
| ZLib          | 1.2.11    | ZLib compression and decompression library: http://www.gzip.org/zlib/ <br>License: [ZLIB Library](https://www.zlib.net/zlib_license.html) |
| ZVBI          | 20160208  | Zapping VBI library: http://zapping.sourceforge.net/ZVBI/ <br>License: [GNU Library General Public License (LGPL) v2.0](https://www.gnu.org/licenses/lgpl-2.0.html) |
