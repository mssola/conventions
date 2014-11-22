
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
for all this is K&R.

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

To do

## Headers

To do.

## Scoping

To do.

## Classes

To do.

## Other C++ Features

To do.

## Encoding

Non-ASCII characters are highly discouraged and they are only allowed in
comments. Use the UTF-8 encoding *always*.

## Comments

To do

