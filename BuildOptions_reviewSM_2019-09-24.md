# Build System

The main build system for SRT is provided by the `cmake` tool, which can be 
used directly. There's also a wrapper script named `configure` (requires Tcl 
interpreter) that can make operating with the options easier.


## Portability

The `cmake` build system was tested on the following platforms:

 - Linux (various flavors)
 - macOS (see this [separate document](build_iOS.md))
 - Windows with MinGW
 - Windows with Microsoft Visual Studio
 - Android (see this [separate document](Android/Compiling.md))
 - Cygwin (only for testing)

The `configure` script wasn't tested on Windows (other than on Cygwin)


## The `configure` Script

This script is similar in design to the Autotools' configure script, and
so usually handles `--long-options`, possibly with values. It handles
two kinds of options:

* special options to be resolved inside the script and that may do some
advanced checks; this should later turn into a set of specific `cmake`
variable declarations

* options that are directly translated to `cmake` variables. 

The direct translate option always does a simple transformation:

* all letters uppercase
* dash into underscore
* plus into X
* when no value is supplied, it defaults to 1

For example, `--enable-c++11` turns into `-DENABLE_CXX11=1` when passed
to `cmake`.

Additionally, if you specify `--disable-<X>`, the `configure` script
will automatically turn it into an associated `--enable-<X>` option,
 passing `0` as its value. For example, `--disable-encryption` will
be translated for `cmake` into `-DENABLE_ENCRYPTION=0`.

### Build Options

All options below are presented using the `configure` convention. They can all
be used in `cmake` with the appropriate format changes.


**`--cygwin-use-posix`** (default:OFF)

When ON, compile on Cygwin using POSIX API (otherwise it will use MinGW environment).


**`--enable-apps`** (default: ON)

Enables compiling user applications.


**`--enable-code-coverage`** (default: OFF)

Enable instrumentation for code coverage. Note that this is only available
on POSIX-compatible platforms.


**`--enable-c++-deps`** (default: OFF)

The `pkg-confg` file (`srt.pc`) will be generated with the `libstdc++` library 
as a dependency. This may be required in some cases where you have an application 
written in C which therefore won't link against `libstdc++` by default.

**`--enable-c++11`** (default: ON)

Enable compiling in C++11 mode for those parts that may require it.
Parts that don't require it will still be compiled in C++03 mode,
although which parts are affected may change in future.

If this option is turned OFF, it affects building a project in two ways:

* an alternative C++03 implementation can be used, if available
* otherwise the component that requires it will be disabled

Parts that currently require C++11 and have no alternative implementation
are:

* unit tests
* user and testing applications (such as `srt-live-transmit`)
* some of the example applications

It should be possible to compile the SRT library without C++11 support. However, 
this alternative C++03 implementation may be unsupported on certain platforms.


**`--enable-debug=<0,1,2>`**

This option allows control through the `CMAKE_BUILD_TYPE` variable:

* 0 (default): `Release` (highly optimized, no debug info)
* 1: `Debug` (not optimized, full debug info)
* 2: `RelWithDebInfo` (highly optimized, but with debug info)

Please note that when the value is other than 0, the
`--enable-heavy-logging` option is also turned ON by default.


**`--enable-encryption`** (default: ON)

Encryption feature enabled, which involves dependency on an external encryption
library (default: openssl). If you disable encryption, the library will be unable
to set encryption options. It will be compatible with a peer that has
encryption enabled, but just won't use encryption for the connection.


**`--enable-getnameinfo`** (default: OFF)

Enables the use of `getnameinfo` in a manner that allows using reverse DNS to 
resolve an internal IP address into a readable internet domain name, so that it 
can be shown nicely in the log file. This option is turned OFF by default 
because it may have an impact on general performance. It is recommended only for
development when testing on a local network.


**`--enable-haicrypt-logging`** (default: OFF)

Enables logging in the *haicrypt* module, which serves as a connector to
an encryption library. Logging here might be seen as unsafe, therefore this 
option is turned OFF by default.


**`--enable-heavy-logging`** (default: OFF in release mode)

This option enables the logging instructions in the code, which are considered
heavy as they occur often and cover every aspect of library behavior. Turning 
this option ON will allow you to use the `debug` level of logging and get 
detailed information as to what happens inside the library. Note, however, that 
this may influence processing by changing times, using less preferred thread 
switching layouts, and generally worsen the functionality and performance of 
the library. For these reasons this option is turned OFF by default.


**`--enable-inet-pton`** (default: ON)

Enables usage of `inet_pton` function by the applications, which should be used
to resolve the network endpoint name into an IP address. This may be not
availabe on some version of Windows, in which case you can turn this OFF,
however at the expense of not being able to resolve IP address by name,
as the `inet_pton` function gets a poor-man's simple replacement that can
only resolve numeric IPv4 addresses.


**`--enable-logging`** (default: ON)

Enables logging. When you turn this option OFF, runtime errors will not be
reported at all. This option may be useful if you suspect the logging system of
impairing performance.


**`--enable-monotonic-clock`** (default: OFF)

This option enforces the use of `clock_gettime` to get the current
time, instead of `gettimeofday`. This function forces the use of a monotonic
clock that is independent of the currently set time in the system. The CV,
for which the `*_timedwait()` functions are used to obtain the current time,
must be appropriately configured. For now, this is only don for the
GarbageCollector controlling CV, not every CV used in SRT. The consequence of
enabling this option, however, may be portability issues based on the
`clock_gettime()` function, which is not available in every SDK, or where an extra
`-lrt` option is sometimes required (this requirement will be autodetected).

>>>
time, instead of `gettimeofday`. This function forces the use of a monotonic
clock that is independent of the currently set time in the system.
The condition variables, for which the `*_timedwait()` functions are used
with time specification basing on the time obtained from `clock_gettime`
must be appropriately configured. For now, this is only done for the
GarbageCollector controlling CV, not every CV used in SRT. The consequence
of enabling this option, however, may be portability issues resulting from
the fact that `clock_gettime` function may be unavailable in some SDKs or an extra
`-lrt` option is sometimes required (this requirement will be autodetected).


<<<



**NOTE:** *This is a temporary fix for Issue #729* where the library could get 
stuck if the system clock is modified during an SRT transmission. This option 
will be removed when the problem is fixed globally.


**`--enable-pktinfo`** (default: OFF)

This option allows the extraction of a target IP address from incoming
UDP packets and the forceful setting of the source IP address in outgoing
UDP packets. This ensures that if a packet comes in from a peer that requests a
new connection, the agent will respond with a UDP packet that has the
same source IP address as the one to which the peer is trying to connect.

When `--enable-pktinfo` is OFF (default), the source IP address in an outgoing 
UDP packet will be set automatically the following way:

* For a given destination IP address in the UDP packet to be sent, find
the routing table that matches this address, then get its network device
and configured network broadcast address.

* Set the **first** local IP address that matches the broadcast
address found above as the source IP address for this UDP packet.

Example: Let's say you have the following local IP addresses:

* 192.168.10.11: broadcast 192.168.10.0
* 10.0.1.15: broadcast 10.0.1.0
* 10.0.1.20: broadcast 10.0.1.0

When a caller is contacting this first address (no matter where it came from), 
the response packet will be sent back to this address. The route path will use 
this first address as well, so the IP address will be 192.168.10.11, same as 
the one that was contacted.

If the caller handshake packet comes from the address that matches the 10.0.1.0 
broadcast (which is common to the second and third addresses), and the target 
address is 10.0.1.20, the response packet will be sent back to this address over 
the network assigned to the 10.0.1.0 broadcast. But the source address will then 
be 10.0.1.15 because it is the first local address assigned to the route path.

When this happens, the caller peer will see a mismatch between the source 
10.0.1.15 address and the address it tried to contact (i.e. 10.0.1.20). It will 
be treated as an attack attempt and rejected.

This problem can be slightly mitigated by binding the listening socket to a 
specific address. So, if you bind it to 10.0.1.20 in the above example, then 
wherever you try to send a packet over that socket it will always have the 
source address 10.0.1.20 (and the fix provided by this option will also not 
apply in this case). However, this problem still exists if the listener socket 
is bound to the "whole machine" (i.e. set to "any" address).

When the `--enable-pktinfo` option is ON, a mechanism is added to forcefully set 
the source IP address in a response packet (to 10.0.1.20 in the above example).
This address is first extracted from the incoming packet as the target address. 
This fixes the problem, as it will be interpreted by the caller peer correctly.

This feature is turned off by default because the impact on performance is 
currently unknown. The problem is that it causes the CMSG information to be read 
from (and set on) every packet when a socket is bound to "any" address. The 
CMSG information will effectively be extracted from every incoming packet, as 
long as it is not bound to a specific address, including data packets coming in 
within the frames of an existing connection.


**`--enable-profile`** (default: OFF)

Enables code instrumentation for profiling.

This is available only for GNU-compatible compilers.


**`--enable-relative-libpath`** (default: OFF)

Enables adding a relative path to a library. This allows applications to be 
linked against a shared SRT library by reaching out to a sibling `../lib` 
directory, provided that the library and applications are installed in POSIX/GNU 
style directories. This might be useful when installing SRT and applications 
in a directory not explicitly defined in the global library path. This option 
is OFF by default because of reports that it may cause problems.


**`--enable-shared`** and **`--enable-static`** (default for both: ON)

Enables building SRT as a shared and/or static library, as required for your 
application. Note that you can't disable both at once.


**`--enable-testing`** (default: OFF)

Enables compiling of developer testing applications.


**`--enable-thread-check`** (default: OFF)

Enables `#include <threadcheck.h>`, which implements `THREAD_*` macros" to 
support better thread debugging.


**`--enable-unittests`** (default: OFF)

When ON, this developer option enables unit tests, possibly with the download 
and installation of the Google test library in the build directory. The tests 
will be run as part of the build process.


**`--openssl-crypto-library=<filepath>`**

Configure the path to an OpenSSL crypto library.


**`--openssl-include-dir=<path>`**

Configure the path to include files for an OpenSSL library.


**`--openssl-ssl-library=<filepath>`**

Configure the path to an OpenSSL SSL library.


**`--pkg-config-executable=<filepath>`**

Configure the path to the `pkg-config` tool.


**`--prefix=<path>`**

This is an alias to `--cmake-install-prefix`. This is the root directory for 
installation, inside which the next GNU/POSIX compatible directory layout will 
be used for specific parts. As on all known build systems, this defaults to 
`/usr/local` on POSIX compatible systems.


>>>

This is an alias to the `--cmake-install-prefix` variable that establishes the 
root directory for installation, inside of which a GNU/POSIX compatible directory layout will be used. As on all known build systems, this defaults to `/usr/local` 
on GNU/POSIX compatible systems, with lower level GNU/POSIX directories 
created inside: `/usr/local/bin`,`/usr/local/lib`, etc.



<<<

**`--pthread-include-dir=<path>`**

Configure the path to include files for a pthread library. Note that this 
applies only to Windows. On Linux and macOS this path should be available in 
the system.


**`--pthread-library=<filepath>`**

Configure the path to a pthread library.


**`--use-busy-waiting`** (default: OFF)

Enable more accurate sending times at the cost of potentially higher CPU load.

This option will cause more empty loop running, which may cause more CPU usage. 
When processing high bitrate streams the share of empty loop runs will decrease 
as the bitrate increases. Without system-supported waiting the option may 
increase the likelihood of switching to the right thread at the time when it is 
expected to be revived.


**`--use-gnustl`**

Use `pkg-config` with the `gnustl` package name to extract the header and 
library path for the C++ standard library (instead of using the compiler
built-in one).


**`--use-enclib=<name>`**

Encryption library to be used. Possible options for `<name>`:

* openssl(default)
* gnutls (with nettle)
* mbedtls


**`--use-openssl-pc`** (default: ON)

Use `pkg-config` to find OpenSSL libraries. You can turn this off to force 
`cmake` to find OpenSSL by its own preferred method.


**`--use-static-libstdc++`** (default: OFF)

Enforces linking the SRT library against the static libstdc++ library. This may 
be useful if you are using SRT library in an environment where it would by 
default link against the wrong version of the C++ standard library, or when the 
library in the version used by the compiler is not available as shared.


**`--with-compiler-prefix=<prefix>`**

Set C/C++ toolchains `<prefix>gcc` and `<prefix>g++`.

This option will override the compiler. It is handled inside `cmake`.
It sets the variables `CMAKE_C_COMPILER` and `CMAKE_CXX_COMPILER`. The given
prefix is then appropriately extended with a suffix dependent on the
compiler type (see `--with-compiler-type`).


**`--with-compiler-type=<name>`**

Sets the compiler type to be used with `--with-compiler-prefix`:

* gcc (default): gcc and g++
* cc: cc and c++
* others: use `<name>` as C compiler and `<name>++` as C++ compiler
