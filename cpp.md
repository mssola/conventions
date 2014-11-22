
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

**Braces** are *always* required. There is no exception to this. Moreover, all
braces have to be followed by a new line character. The only exception to this
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
of STL functions (e.g. `push_back`), but I think that is better to stick with
one single naming convention for functions always.

### Namespaces and Classes

Use `lowercase` for namespaces and `CamelCase` for classes. The rationale
for this is that if we picked the same naming convention, then things like
`Something::foo` might be confusing, since we would not know if `Something` was
a namespace or a class. Moreover, namespaces follow the `lowercase` convention
because I believe that namespaces should be named with a simple word, rather
than a compound word. Since we generally go for `lowerCamelCase`, for simple
words this just results into `lowercase`.

### Enumerator Names

Enumerator names and their list of values should follow the `CamelCase` naming
convention.

### Macros

Use `UPPER_CASE_WITH_UNDERSCORE`. The rationale behind this is that macros
should be easily noticeable and scare people, so they deserve an frightning
naming convention.

## Headers

To do.

## Scoping

To do.

## Classes

To do.

## Other C++ Features

To do.

## Language & Encoding

- Use English always.
- Non-ASCII characters are highly discouraged and they are only allowed in
comments.
- Use the UTF-8 encoding *always*.

## Comments

To do

