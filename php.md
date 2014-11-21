
## PHP

I'm basically following the MediaWiki conventions. You can find them here:

1. [General Code Conventions](http://www.mediawiki.org/wiki/Manual:Coding_conventions)
2. [Code Conventions for PHP](http://www.mediawiki.org/wiki/Manual:Coding_conventions/PHP)

The first one specifies the coding conventions in general, the second one the
specifics of PHP. I find them all pretty agreeable except for the rules on
braces. In regards to braces, [K&R](https://en.wikipedia.org/wiki/Indent_style#K.26R_style)
were right, period. This applies to PHP in the following way:

    Always place the opening brace in the same line except for class,
    interface, trait and function declarations. In these cases, the opening
    brace has to be placed in the next line.

Besides this, the MediaWiki team is on point on every other detail, so follow
their conventions. Moreover, I'm also adding the following points:

- Be as verbose as possible when specifying language features. This includes
things like adding keywords such as `final`, access modifiers, type hints, etc.
- Always use the short array syntax:

```php
$a = [1, 2];      // yes.
$a = array(1, 2); // no.
```

- Prefer the lowercase version of built-in values. For example:

```php
$a = null; // not NULL.
$a = true; // not TRUE.
```

