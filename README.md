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


