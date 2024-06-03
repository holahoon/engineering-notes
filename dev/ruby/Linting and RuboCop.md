Formatting is all about making your code look neat and tidy without changing code’s behavior - think indentation and various spacing, so style. Linting is all about making your code easier to reason about - this might change how your code behaves, for example by enforcing that you use `#each` instead of a `for` loop. For all that, Rubyists have a powerful ally:

RuboCop. A really polished Gem that will make your code shine!

As a starting point, lets use some of RuboCop’s departments - Style, Lint and Metrics. Perhaps you’ve anticipated that RuboCop doesn’t work alone - it indeed has a whole precinct behind itself. The various Cops are interested in making sure that some particular rule is not broken. Let the departments speak for themselves:

 >Style Cops check for stylistic consistency of your code. Many of the them are based on the Ruby Style Guide.
 >
> Lint Cops check for ambiguities and possible errors in your code.
> 
> Metrics Cops deal with properties of the source code that can be measured, such as class length, method length, etc.

## Learn all about code crime

RuboCop is a Gem and the project we want to investigate is Caesar Cipher. Go back and install RuboCop locally (as in, use Bundler) and then run `bundle exec rubocop` in your terminal. Running it like this makes sure that the local version of RuboCop is used and it will check all the files in the current working directory _and_ its subdirectories. In short: everything.

Let's take a look at this example output by RuboCop:
```bash
Inspecting 2 files
CC

Offenses:

Gemfile:3:8: C: [Correctable] Style/StringLiterals: Prefer single-quoted strings when you don\'t need string interpolation or special symbols.
source "https://rubygems.org"
       ^^^^^^^^^^^^^^^^^^^^^^
caesars_cipher.rb:1:1: C: [Correctable] Style/FrozenStringLiteralComment: Missing frozen string literal comment.
def caesar_cipher(string, shift_factor)
^
caesars_cipher.rb:3:5: C: [Correctable] Layout/EmptyLineAfterGuardClause: Add empty line after guard clause.
    return string if shift_factor.remainder(26) == 0
    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
caesars_cipher.rb:3:22: C: [Correctable] Style/NumericPredicate: Use shift_factor.remainder(26).zero? instead of shift_factor.remainder(26) == 0.
    return string if shift_factor.remainder(26) == 0
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
caesars_cipher.rb:15:5: C: [Correctable] Style/RedundantReturn: Redundant return detected.
    return character
    ^^^^^^
caesars_cipher.rb:16:8: C: [Correctable] Style/StringLiterals: Prefer single-quoted strings when you don\'t need string interpolation or special symbols.
  when "z"
       ^^^

2 files inspected, 19 offenses detected, 19 offenses autocorrectable

# Duplicate offenses in the same file were truncated.

```

Let's break this down. The output starts with telling how many files are to be inspected:
```bash
Inspecting 2 files
```

Then, a somewhat mysterious string of capital letters appeared: `CC`:
```bash
CC
```

Every letter corresponds to one file. So both of the files inspected by RuboCop reported “convention” as their most severe issues. The other letters that you might want to know are `W` for warning and `F` for fatal. Finally, the output arrives at the crime scene - offenses:
```bash
Offenses:

Gemfile:3:8: C: [Correctable] Style/StringLiterals: Prefer single-quoted strings when you don\'t need string interpolation or special symbols.
source "https://rubygems.org"
       ^^^^^^^^^^^^^^^^^^^^^^
```

The output resembles stack traces a little bit: you get the file, the line and the column, then severity level letter. After that you receive information on whether RuboCop can fix the problem on its own. We’ll get back to this very soon. Further, you learn of the Department/Cop - in this case, you’re dealing with StringLiterals Cop from the Style department. Obviously, our cybernetic assistants are polite enough to introduce themselves, so you’re being told what is wrong. Finally, you are directly told what and where is the offending part of code. Here, RuboCop pointed out that this string could very well be single-quoted, as that string doesn’t use anything that comes with double-quoted strings.

You could ask why such a foundational Gem as Bundler can run into trouble with the law. The answer is that RuboCop is highly customizable to accommodate many standards that programmers might have. Remember: what is important in linting and formatting is making sure everyone in a group plays by the same rules, so the code is more readable.

Before you unleash the automated fury of RuboCop upon your code, you might want to become acquainted with a nifty flag: `-S`. This will provide a link to the Ruby Style Guide that goes over the rationale for the offense, if the Cop has such link when `bundle exec rubocop -S` is used:
```bash
Gemfile:3:8: C: [Correctable] Style/StringLiterals: Prefer single-quoted strings when you don\'t need string interpolation or special symbols. (https://rubystyle.guide#consistent-string-literals)
source "https://rubygems.org"
       ^^^^^^^^^^^^^^^^^^^^^^
```
Nifty, eh?

But I hear you - you’re here for action. So let’s go for the `-a` flag, a for action! (Actually, it stands for autocorrect but that’s not as fun) `bundle exec rubocop -a`, go!
```bash
Inspecting 2 files
.C

Offenses:

caesars_cipher.rb:1:1: C: [Correctable] Style/FrozenStringLiteralComment: Missing frozen string literal comment.
def caesar_cipher(string, shift_factor)
^
caesars_cipher.rb:3:5: C: [Corrected] Layout/EmptyLineAfterGuardClause: Add empty line after guard clause.
    return string if shift_factor.remainder(26) == 0
    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
caesars_cipher.rb:3:22: C: [Correctable] Style/NumericPredicate: Use shift_factor.remainder(26).zero? instead of shift_factor.remainder(26) == 0.
    return string if shift_factor.remainder(26) == 0
                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
caesars_cipher.rb:15:5: C: [Corrected] Style/RedundantReturn: Redundant return detected.
    return character
    ^^^^^^
caesars_cipher.rb:16:8: C: [Corrected] Style/StringLiterals: Prefer single-quoted strings when you don\'t need string interpolation or special symbols.
  when "z"
       ^^^

2 files inspected, 16 offenses detected, 14 offenses corrected, 2 more offenses can be corrected with rubocop -A
# Duplicate offenses were truncated.
```

See that `.`? That means the first file is now all fine and dandy! Some of the offenses were not corrected by RuboCop and that’s because `-a` is for safe autocorrect. If you wanted to go through with the `[Correctable]` offenses, you’d want to use `-A` as the output helpfully suggests. This is due to the fact that some Cops are safe, some are unsafe.

The safe Cops promise that they won’t have false positives and that their autocorrect won’t change the semantics of the code and it will be fully equivalent to what you had written.

From this it follows that unsafe Cops either have false positives or slightly change the semantics of the code. The first characteristic means they tell you something is wrong when actually, everything is alright - for example, you’ve got a method with the same name as one in standard library in your object and RuboCop treats it as if it were the standard library method.

The latter means that while your code and the proposed code arrive at the same output, they might change how (but not what) the output is achieved or produce a side-effect that might even break your code.

## You are the code dictator

Due to Ruby’s ecosystem, RuboCop was built with extensive configurability in mind - both in terms of not using some parts of and in terms of adding onto it. Every single Cop can be disabled, sometimes Cops offer alternative rules like preferring single- or double-quotes for Strings, you can disable Cops on a per-file basis and much more.

Since RuboCop is extensible, there exist other departments that you can use - like Performance or RSpec. You could even write your own Cop! The process of adding an extension is easy: you install the Gem locally and modify `.rubocop.yml`.

Usually things that are not required for app to run are given the `require: false` flag, like:
```bash
`gem 'rubocop-performance', require: false`
```

This way the Gem would be installed normally, but for your `bundle exec` ran code to make use of it, it would need to be explicitly `require`d wherever you’d need it.

`.rubocop.yml` is the configuration file for RuboCop and it lives in the root directory of your project. There you’ll change the defaults of RuboCop to your (or most likely, your team’s) liking. To create such config file, you can just use `bundle exec rubocop --init` - it won’t have anything in it besides a comment describing what it is for but if we were to add the Performance extension, we’d need to throw `require: rubocop-performance` in there so RuboCop knows to run it.

RuboCop is still under development, so changes and additions happen. New Cops join the precinct and they’re not enabled by default - if you’d like them to be enabled by default instead of going through all of them and deciding on your own, you can use:

```bash
AllCops:
  NewCops: enable
```

in your `.rubocop.yml` to enable all the new Cops.







