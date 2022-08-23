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


- [Academic Free License (AFL) v3.0](https://opensource.org/licenses/AFL-3.0)
- [Apache Sofware License (ASL) v2.0](https://www.apache.org/licenses/LICENSE-2.0)
- [Common Public License (CPL) v1.0](https://opensource.org/licenses/CPL-1.0)
- [Berkeley Software Distribution (BSD) Style License](https://en.wikipedia.org/wiki/BSD_licenses)
- [BOOST Software License v1.0](https://www.boost.org/LICENSE_1_0.txt)
- [GCC Runtime Library Exception](https://www.gnu.org/licenses/gcc-exception-3.1.en.html)
- [GNU Linking Exception](https://en.wikipedia.org/wiki/GPL_linking_exception)
- [GNU General Public License (GPL)](https://www.gnu.org/licenses/licenses.html#GPL)
- [GNU General Public License (GPL) v3.0](https://www.gnu.org/licenses/gpl-3.0.en.html)
- [GNU Library General Public License (LGPL)](https://en.wikipedia.org/wiki/GNU_Library_General_Public_License)
- [GNU Library General Public License (LGPL) v2.0](https://www.gnu.org/licenses/lgpl-2.0.html)
- [GNU Library General Public License (LGPL) v2.1](https://www.gnu.org/licenses/lgpl-2.1.html)
- [GNU Library General Public License (LGPL) v3.0](https://www.gnu.org/licenses/lgpl-3.0.html)
- [Internet Systems Consortium (ISC) License](https://opensource.org/licenses/ISC)
- [LIBSDTC++ Runtime Library Exception](https://gcc.gnu.org/onlinedocs/libstdc++/manual/license.html)
- [LIBSDTC++ Runtime Library Exception FAQ](https://gcc.gnu.org/onlinedocs/libstdc++/faq.html#faq.license.what)
- [Mozilla Public License (MPL)](https://www.mozilla.org/en-US/MPL/)
- [Public Domain License](https://en.wikipedia.org/wiki/Public_domain)
- [Massachusetts Institute of Technology (MIT) License](https://en.wikipedia.org/wiki/MIT_License)
- [FLTK Library License](https://www.fltk.org/doc-1.4/license.html)
- [ZLIB Library](https://www.zlib.net/zlib_license.html)

