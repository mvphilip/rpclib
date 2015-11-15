# callme

`callme` may be the fastest MsgPack-RPC library for C++! It is built using modern C++14,
and as such, requires a very recent compiler.

The library uses a recent (continually updated) version of the msgpack headers. I decided
to not require it as a dependency but rather keep it in the repo, because it is quite small
and header-only.

## Why another implementation?

The [original implementation for C++](https://github.com/msgpack-rpc/msgpack-rpc-cpp) was
discontinued. It lives on as [jubatus-msgpack-rpc](https://github.com/jubatus/jubatus-msgpack-rpc/tree/master/cpp), but this implemenation has a couple of undesirable requirements (which make perfect sense in the context of the library as part of Jubatus).

  * Autoconf-based build. Not only autoconf is dated, it also pretty much requires a Unix platfrom.
    Yes, cygwin is a workable alternative, but it is not completely native on windows, and means no
    MSVC support.
  * Lesser-known dependencies (it uses MPIO for the async event loop, which is yet another
    autoconf-based build, also part of Jubatus).
  * Awkward server implementation. The server implementor is required to unpack the parameters
    manually and dispatch the call based on the name of the methods.
  * At this moment (November 2015) it [doesn't compile](https://github.com/jubatus/jubatus-msgpack-rpc/issues/21) with the latest msgpack library.
  * Somewhat lacking documentation.

`callme` tackles these issues the following way:

  * CMake build. Even though [not the most pleasant build system](http://szelei.me/cmake-is-not-great/) to work with, it provides a reliably reproducible build for the end-user.
  * Self-contained dependencies. This is a trade-off, but I felt that the ease of build tramps the
    ability to easily choose different versions of dependencies.
  * Modern interface which provides minimalistic binding for many kinds of functors, allowing the
    library to dispatch functions automatically.
  * Extensive documentation.

On top of that, it also provides a code generator utility which can generate strongly typed,
"batteries included" clients and servers from a class header file.
