
## Formatting

All indentation is to be done using **tabs**. Each tab is 4 spaces wide. With
this in mind, lines should not be longer than 80 characters, unless absolutely
required for some syntactic reason. Of course, lines should never have any
trailing whitespace.

A new indentation level is added after an opening brace that is followed by a
new line character, but there are some special exceptions to that:

- `case` statements inside of a `switch` statement may not be further indented.
As an example:

```cpp
switch (var) {
case 1:
    ...
    break;
default:
    ...
}
```

- We will never add an indentation level after opening a `namespace`. Example:

```cpp
namespace thing {

int a = 0;

}
```

- `Access specifiers` (public, protected and private) are indented in the same
level as the line containing the `class` or the `struct` keyword. So, for
example:

```cpp
class MyClass
{
    // No!
    public:

// Yes
public:
}
```

**Braces** are *always* required. There is no exception to this. Moreover, all
braces have to be followed by a new line character. The only exceptions to this
are `else` statements, `do-while` statements and exceptions (but exceptions
are forbidden anyways). Therefore:

```cpp
// No! Braces and a new line character are missing.
if (condition) statement;

// No! Braces are missing.
if (condition)
  statement;

// No! Else statements have to be in the same line as the closing brace.
if (condition) {
  statement;
}
else {
  statement;
}

// Yes
if (condition) {
  statement;
} else {
  statement;
}
```

Moreover, we don't put braces in a new line except for functions and classes.
So:

```cpp
// No
while (condition)
{
  ...
}

// Yes
while (condition) {
}

// No
void foo() {
}

// Yes
void foo()
{
}
```

The rationale for this is K&R. Classes are treated as functions in this case
because that's how I do it (yes, I don't have any clever excuse for this).

Use one **space** after each keyword. For pointers and references, use a
single space before `*` or `&`, but not after. All binary operators should have
one space on each side, except for `.`, `->`, `.*` and `->*`. Unary operators
have one single space before them, but zero spaces after. Postfix increment &
decrement operators don't have any space before them and, conversely, prefix
increment & decrement operators don't have any space after them. The rationale
for all this is K&R. Some examples:

```cpp
// Neither of both are ok.
MyClass* obj;
MyClass * obj;

// Ok.
MyClass *obj;
```

If one expression does not fit on one line, attempt to wrap it after an
operator and indent subsequent lines with the beggining of the current
parenthesis/brace nesting level. For example:

```cpp
if (someCondition &&
    anotherCondition) {
}
```

Function calls and function declarations follow the same previous rule if they
don't fit the current line. Example:

```cpp
// Ok.
functionCall(arg1, arg2,
              arg3, arg4);

// Also ok.
functionCall(
    arg1, arg2,
    arg3, arg4);

// Also Ok.
functionCall(
    arg1, arg2,
    arg3, arg4
);

// Ok.
void function(int arg1,
              int arg2)
{
}
```

If an initializer list can be kept on a single line, it is fine to do so:

```cpp
MyClass::MyClass(int idx) : m_idx(idx)
{
}
```

Otherwise, the proper way of formatting it is to put each member in a new line.
Each comma has to be at the beginning of each line (except for the first
member, of course). For example:

```cpp
MyClass::MyClass(int idx, int another, int yetAnother)
  : m_idx(idx)
  , m_another(another)
  , m_yetAnother(yetAnother)
{
}
```

## Naming

### Variables

Use `lowerCamelCase` for all local variables. Static variables should
additionally be prefixed by `s_`. Likewise, global variables should be prefixed
by `g_`.

Try to use descriptive and short names. For local variables inside a short
function, it might be just fine to use a variable named `it` for a `for` loop
iterator. However, if this function is a bit large and contains multiple loops,
maybe it is not such a great idea to name it `it` (unless you somehow re-use
this variable). Even though I believe that common sense is the least common of
all senses, try to use it when naming variables.

### Constants

All constants should be prefixed with `k` and use `CamelCase`. For example:
`kInvalidHandle`. Moreover, prefer `constexpr` to `const` whenever possible.

### Struct & Class data members

As with variables, use `lowerCamelCase` for all data members. Additionally,
private instance members should be prefixed with `m_` (e.g., `m_cls`), and all
static members should be prefixed with `s_` (e.g., `s_instance`). Public
members should be left unprefixed.

### Functions

Use `lowerCamelCase` always. It might be confusing if the file contains a lot
of standard functions (e.g. `push_back`), but I think that is better to stick
with one single naming convention for functions always.

### Namespaces and Classes

Use `lowercase` for namespaces and `CamelCase` for classes. The rationale
for this is that if we picked the same naming convention, then things like
`Something::foo` might be confusing, since we would not know if `Something` was
a namespace or a class. Moreover, namespaces follow the `lowercase` convention
because I believe that namespaces should be named with a simple word, rather
than a compound word. Since we generally go for `lowerCamelCase`, for simple
words this just results into `lowercase`.

### Enumerator Names

Enumerators and their list of values should follow the `CamelCase` naming
convention.

### Macros

Use `UPPER_CASE_WITH_UNDERSCORES`. The rationale behind this is that macros
should be easily noticeable and scary, so they deserve a frightning naming
convention.

## Headers

Every `.cpp` file should have a corresponding `.h` file, with the same name,
and which declares its public interfaces. All the declarations inside the `.h`
file have to be properly documented. Also try not to make large header files
that just serve to include groups of large header files. In general, it is
always preferrable that each class has its own `.h` file, but I do not have any
strong feelings about this either, it is just a suggestion.

All headers should have define guards. Define guards should have a proper name
but more importantly, they all should have the same naming convention. As a
suggestion, I usually do:

```cpp
#ifndef <NAMESPACE>_<CLASS_NAME>_H_
#define <NAMESPACE>_<CLASS_NAME>_H_

// Code

#endif /* <NAMESPACE>_<CLASS_NAME>_H_ */
```

Header files should be *self-contained*. That is, header files should not rely
on the list of headers included by included headers. To help with this, you can
follow these guidelines:

- Always include the corresponding `.h` for a `.cpp` first.
- Separate includes into groups: C++ standard library headers, external
projects (such as `Boost` and `Qt`), and finally headers within your project.
Each group should be separated by a newline.
- Keep headers alphabetized within each group.
- Use angle brackets always.
- Relative paths are forbidden.
- Use class/struct forwarding whenever possible, to avoid unneeded includes.
- Some libraries such as `Qt` offer multiple ways of including library headers.
In these cases, always be as explicit as possible in regards to the path (e.g.
`<QtCore/QString>` instead of just `<QString>`).

As an example, an hypothetical `MyClass` might have the following include
section in its `.cpp` file:

```cpp
#include <myclass.h>

#include <string>
#include <vector>

#include <QtCore/QString>

#include <aclass.h>
#include <anotherclass.h>
#include <yetanotherclass.h>
```

## Scoping

### Namespaces

- All the code has to be scoped by a common namespace. The name of this common
namespace should be related to the project's name.
- It is ok to have nested named namespaces if they improve the code hierarchy.
- Anonymous namespaces are encouraged in `.cpp` files. The rationale behind
this is that they avoid link time naming conflicts by keeping symbols internal
to their translation unit. They are better than using the `static` keyword
because `static` does not work on struct and class declarations.
- Inline namespaces are forbidden. Just don't do it, they are not worth it.
- The `using` directive is forbidden in `.h` files. It is fine to use them in
`.cpp` files but try to use them carefully. The rationale is that they pollute
the current namespace: for `.cpp` files it might be useful, but for `.h` files
it is just pure evil.

### Static and Global scopes

Static and global variables are highly discouraged. When declaring a static or
a global variable you have to write a comment with a convincing argument in it.
The rationale is that static and global variables are one of those things that
can bite you in the ass in unexpected ways. Moreover, it is always hard to find
and fix bugs that are related to static and global variables.

Things are a bit different here in regards to functions. You may use global
nonmember functions *if* they are contained in an anonymous namespace.
Otherwise, you should use completely global functions rarely.

### Nesting

In C++ you can nest a lot of things. In particular:

- Namespaces: as previously explained, it is fine to nest multiple namespaces
if you think that it improves the code hierarchy.
- Classes: please don't. Nesting classes is not banned, but it is highly
discouraged. The rationale behind this is that there is often better ways to
solve what nested classes do (e.g. forward a class declaration and then declare
it on an anonymous namespace in the `.cpp` file).
- Functions: nested functions are prohibited. Use `lambdas` instead.

## Classes

### Using struct vs class

In C++ `struct` and `class` have nearly identical meanings; the only difference
lies in the default accessibility (`struct` defaults to public, and `class`, to
private).

I do not have any strong arguments in regards when to use `struct` and when
`class`. As a suggestion:

- Use `struct` for "passive" objects. That is, for objects that just carry data
but lack any functionality other than accessing/setting the data members.
- If more functionality is needed, then use `class`.
- When in doubt, use `class`.

### Implicit and explicit constructors

By default, always use `explicit` for constructors callable with one argument.
This also applies to constructors where every parameter after the first has a
default value. The rationale behind this is that with this we avoid nasty
implicit conversions. Using `explicit` for constructors with no arguments or
for constructors with more than one argument with no default value, is
pointless.

### Getters & setters

Prefer declaring public member variables to using getters and setters. Getters
and setters that do not manage object state in a nontrivial way serve to bloat
the API and introduce unnecessary boilerplate.

Getters are, of course, encouraged for private members. Avoid prefixing getters
with `get`.

### Inlining

Defining functions with the `inline` keyword is encouraged if said function is
very short (e.g. 1-3 lines of code). Moreover, writing `inline` is always
required, even if functions implemented in `.h` files are [always inlined by
the compiler](http://stackoverflow.com/a/3343046).

### Friends

Using the `friend` construct is allowed, within reason. Using this construct
might be useful in some particular cases, but generally speaking, its usage is
discouraged.

### Overloading

In my opinion, overloading operators is a rather dangerous thing to do. So, the
general rule for this is to not do it at all. There might be some special
exceptions to this (e.g. a logging class that emulates the `std::cout` API by
overloading the `<<` operator).

### Declaration order

Adhere to the following order for declarations in a `struct` or a `class`
definition:

1. Friend classes.
2. Nested enums, typedefs and aliases (through the `using` keyword).
3. Public, protected and private methods, in this specific order.
4. Constants and static data members.
5. Public, protected and private instance data members, in this specific order.

The list of methods and members might be grouped, for readability. Each
subgroup should be separated from each other with a blank line.

## Other Features

### Casting

- Avoid C-style casting, favoring `static_cast`. It is safer.
- Try to avoid the `Run-Time Type Information` with things like `dynamic_cast`.
Doing this is prone to abuse and it has performance issues.

### Exceptions

They are forbidden. In this case I agree completely with the Go programming
language: exceptions should be for exceptional cases (no pun intended), and in
these cases what you want to do is to exit the program (`panic` in Go's
terminology). Otherwise, we are just dealing with errors, and in a safe
environment functions should always check for errors, so there is no point of
using exceptions in my humble opinion.

This prohibition also applies to exception-related features added in C++11,
such as `std::exception_ptr` and friends. Note, however, that you might use
the new keyword `noexcept` if you are sure that the compiler will perform
optimizations on top of it (which is questionable).

### Enums

Prefer `enum class` whenever possible. Old-style `enum` is not forbidden, but
its usage is discouraged. The rationale is that `enum class` is strongly
typed and `enum` is not, so they are safer for the purpose of enumerating
things.

### rvalue references

To do.

### C++11 and C++1y Features

Let's talk about some C++11 and C++1y features that have not been dealt in
other sections of this document:

- Use `nullptr` for pointers always. It is safer.
- Use `auto` when writing the whole type adds clutter (e.g. iterators). Never
use `auto` for anything but local variables.
- Always use `override` when overriding a method in a subclass. It is safer.
- Prefer C++11 foreach syntax to explicit iterators:

```cpp
for (const auto &it : myVec) {
    ...
}
```

### Other

There are some controversial language constructs such as `goto` or the
`operator,()`. My take on this is that they are accepted but highly
discouraged. That is, you can use them, but you better have a convincing
argument when using them rather than using another C++ construct.

## Language & Encoding

- Use English always.
- Non-ASCII characters are highly discouraged and they are only allowed in
comments.
- Use the UTF-8 encoding *always*.

## Comments

To do

## Influencers for this coding convention

This coding convention is largely based on the coding conventions used at
[Google](http://google-styleguide.googlecode.com/svn/trunk/cppguide.html) and
the
[HHVM](https://github.com/facebook/hhvm/blob/master/hphp/doc/coding-conventions.md)
project. In fact, I have even *copy-pasted* text in which I agreed completely.

Other influencers:

- [LLVM](http://llvm.org/docs/CodingStandards.html).
- [KDE](https://techbase.kde.org/Policies/Kdelibs_Coding_Style).
- [Linux](https://www.kernel.org/doc/Documentation/CodingStyle).

