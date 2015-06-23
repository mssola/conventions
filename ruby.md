
## Ruby

In Ruby there are two popular style guides:

1. [Github](https://github.com/styleguide/ruby).
2. [Community-driven guide from
bbatsov](https://github.com/bbatsov/ruby-style-guide).

Personally, I'm all for the second option. The community-driven guide is
*extensive* and has all the good practices that all rubyist should follow. I
only have one objection: Perl-style global variables are accepted. I like Perl,
and even though I agree that Perl-style global variables are not very
descriptive, I'm too used to them.

## rubocop

In order to enforce these style guide in our code, you can use
[rubocop](https://github.com/bbatsov/rubocop). The default behavior of
`rubocop` follows the community-driven guide as close as possible. Moreover,
I've also added a
[rubocop.yml]((https://github.com/mssola/conventions/blob/master/files/rubocop.yml)
file that fixes some of the defaults.

