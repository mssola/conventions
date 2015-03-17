
## General

**-Werror**: a bit hardcore, but warnings are usually a bad thing. Sometimes it
happens that some components are auto-generated (e.g. through `bison`) and they
bring warnings. We can solve this by explicitly disabling some warnings for a
specific set of files. For example, in CMake we can do something like:

```cmake
set_source_files_properties(${CMAKE_CURRENT_BINARY_DIR}/parser.cpp
  PROPERTIES COMPILE_FLAGS
  "-Wno-sign-conversion -Wno-old-style-cast -Wno-switch-default -Wno-unreachable-code")
```

**-Wall**, **-Wextra**: use it always since it includes the vast majority of
warnings, and all of them are useful.

**-Wpedantic**: issue all warnings demanded by strict ISO C and ISO C++. It also
disables some GNU extensions. This is ok, because even if clang implemented
most of the GNU extensions, we really should try to comply with the ISO as much
as possible. A pretty simple (and stupid) example of code rejected by this
warning is:

```cpp
int a;; // <- double ';'
```

**-Wmissing-declarations**: it shows a warning for globally defined functions
that have not been declared before (e.g. in a header file). It should not
affect us, because global variables are prohibited in my code style. It can
happen if we have let's say a `main.cpp` file and we have some helper
functions there. This can be solved with a simple anonymous namespace.

**-Wshadow**: it can be useful but it has some caveats. A common problem for
me was:

```cpp
// In the .h file:

struct Thing {
  Thing(std::string name);

  std::string name;
};

// In the .cpp file:

Thing::Thing(std::string name) : name(name)
{
}
```

This will result in a warning because in the initializer list there's a
shadowing with the `name` variable. One way to solve this is to rename the
variable on the function signature of the `.cpp` file just like this:

```
Thing::Thing(std::string n) : name(n)
{
}
```

With this we no longer have a warning. Since `n` is assigned to `name`, it's
clear that `n` is the name without looking at the .h file.

**-Wswitch-default**: this is probably not the most important one, but it forces
us to handle default cases. That is, even if we think that we have handled every
single case in a switch, there will always be *that* case that we didn't think
was possible or that we just forgot.

**-Wundef**: don't evaluate undefined stuff in #if directives. This is weird,
and I think I never encountered this, but it's better to be safe than sorry.

**-Winit-self**: this is pretty stupid, but ironically enough, I've encountered
this in some code bases. This warning tells when a variable is being
initialized by itself. Example:

```cpp
int i = i;
```

**-Wmissing-include-dirs**: dear user, give me valid include directories.

**-Wredundant-decls**: show a warning for redundant declarations, even if these
redundant declarations are valid in C++. This warning is more about code style
than anything else. Example of a valid redundant declaration invalidated by
this warning:

```cpp
void foo();
void foo(); // <-- redundant!

void foo()
{
}
```

**-Wunreachable-code**: sometimes the compiler can guess that a certain portion
of code will never be executed. This warning is useful to detect such cases.
Example:

```cpp
void foo()
{
  goto a;
  std::cout << "Never executed!!" << std::endl;
a:
  std::cout << "Bye bye" << std::endl;
}
```

**-Wmissing-noreturn**: sometimes the compiler can hint that in some places the
`[[noreturn]]` attribute could be applied. With this warning activated, the
compiler will help us on this regard.

## Class & struct

**-Wctor-dtor-privacy**: show a warning when it appears that a class has no
usable constructor. I've been in this situation when for some reason I forgot
to write an access specifier. Example:

```cpp
class Thing {
    // This class has no public constructors!!
    Thing() {}
};
```

**-Wnon-virtual-dtor**: raise a warning when a class does not define a virtual
destructor and it should (e.g. it has other virtual methods). This way we
prevent some unsafe de-allocations.

**-Woverloaded-virtual**: warn us when a function declaration hides virtual
functions from a base class. Example:

```cpp
struct Base {
  virtual void foo();
};

struct Class : public Base {
  void foo();
};
```

## Castings & conversions

The following warnings can be summed up as: "Compiler, be as rigid as possible
here, and don't allow fuck-ups with castings and signedness".

**-Wcast-align**: this warning is important because it prevents fuck-ups when it
comes to dangerous castings that can change alignments (e.g. casting similar
types with different alignments in some weird architecture).

**-Wsign-promo**: prevent the promotion of unsigned-signed-enum types. We don't
want to mess with signedness.

**-Wcast-qual**: raise a warning for casts that remove the type qualifier for
the target type. This also applies when we are adding a type qualifier in an
unsafe way. Example:

```cpp
char **a;
const char **b = (const char **) a;
```

**-Wold-style-cast**: in our style guide we prefer static_cast & co. instead of
old C style casts.

**-Wsign-conversion**: don't mess with signedness, since it can be tricky on some
platforms. It's incredibly easy to mix signed and unsigned types, and we don't
want that.

## Compiler-specific warnings

**-Wlogical-op**: this warning is only available in GCC, and it will tell us
suspicious uses of logical operators, parenthesis, etc. This is especially
useful for codebases where the author didn't care about parenthesis and such.
Yes, I know about operator precedence, but let's be nice and not
over-complicate things that can be simple.

## Flags

**-std**: try to use the most recent version of ISO C and ISO C++ depending on
your compiler requirements. Usually compiler developers provide a site in
which you can check what's the status of the implementation of multiple
versions of ISO standards:

    - **Clang**: [C++11, C++14, C++1z](http://clang.llvm.org/cxx_status.html)
    - **GCC**: [C++14](https://gcc.gnu.org/projects/cxx1y.html)

You should also check which compiler versions are available on the Operating
Systems that you're planning to support. All in all: yes, it's a pain in the
ass to check all this.

**-fno-exceptions**: according to my style guide, exceptions are forbidden, so tell
the compiler that we don't need them.

