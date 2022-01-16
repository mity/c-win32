
# C-Win32 Readme


## Overview

This "project" provides some header files which are moral replacements for
corresponding Win32API headers from Microsoft Windows SDK which require C++
and which are, as they are, incompatible with plain C.

These headers certainly are not meant as a drop-in replacement for a whole SDK.
We only allow, by using a particular replacement header, to use a particular
SDK functionality which would otherwise be unusable from C.


## Disclaimer

Be warned that these replacement headers are **very incomplete**: They contain
only the stuff I have so far needed in my projects. Their instant usability in
your projects can be limited or require some work on your side.

That said, if you like the approach and find the headers useful, adding more
stuff is usually quite easy and we are open to your pull requests.

However as a rule of thumb, please follow **the following rule**: Add only such
stuff you really use in your code. This is especially important for COM vtables
which may be quite complicated and where any subtle error can introduce a binary
incompatibility and hard-to-debug issues when used.

For example, when adding a new COM vtable, use just some dummy members for the
methods you do never call, just as placeholders in the vtable structure.
Similarly provide the convenient wrapping preprocessor macros only for those
methods you use (i.e. those which are not the placeholders.

(See how the headers do this already.)


## Naming Conventions

Our headers have the same names as original ones, only with the prefix "c-".
E.g. instead of `#include <gdiplus.h>`, use `#include <c-gdiplus.h>` in your
code.

Similarly, to avoid clashes with the standard identifiers (as some of them may
have forward declarations in other headers), all global identifiers provided by
our headers use the prefix `c_`.

That includes:
  - names of preprocessor macros,
  - names of enumerations, structures, unions or other types, and
  - members of enumerations (as in C those are also globally visible).

(Non-member) functions may be more tricky because on one side we want them to
be easy to use without much extra type casting and at the same time, their name
must match the original function name, as exported by the target system DLL.

Fortunately, we have needed this so far only in very few cases (as we mainly
replace headers providing COM interfaces or interfaces quite similar to COM as
GDI+). Therefore, we could solve these individually in what was seen as a
reasonable trade-off in any particular situation.

(If a need to add more function prototypes arises we may need to define some
common way how to do that.)


## Using C-Win32

1. Just copy the needed headers into your project or instruct your compiler
   where to find the headers, typically via `-Ipath/to/c-win32/include` command
   line option.

2. Include the replacement header instead of the vanilla one. E.g.:
   ```
   #include <c-gdiplus.h>
   ```

3. Use the prefix `c_` when referring to any global type or other global
   identifier provided by the header.

The following projects use c-win32, so you can take a look how to use the
replacement headers:

* [mCtrl](https://github.com/mity/mctrl)
* [WinDrawLib](https://github.com/mity/windrawlib)


## License

These headers are covered with MIT license, see the file `LICENSE.md`.
