# globbing

> Cheatsheet and introduction to "globbing".

**TODO**

* [ ] cheatsheet
* [ ] what is globbing
* [ ] wildcards
* [ ] extglobs
* [ ] posix brackets
* [ ] braces
* [ ] options

<details>
<summary><strong>Table of contents</strong></summary>

- [Contributing](#contributing)
- [What is "globbing"?](#what-is-globbing)
- [Wildcards](#wildcards)
- [Extended globbing](#extended-globbing)
  * [brace expansion](#brace-expansion)
- [extglobs](#extglobs)
  * [POSIX character classes](#posix-character-classes)
  * [Regular expressions](#regular-expressions)
    + [regex character classes](#regex-character-classes)
    + [regex groups](#regex-groups)
- [Common options](#common-options)
- [Risks](#risks)
- [Related concepts](#related-concepts)
- [Resources](#resources)

_(TOC generated by [verb](https://github.com/verbose/verb) using [markdown-toc](https://github.com/jonschlinkert/markdown-toc))_

</details>

## Contributing

We love contributors!

[Pull requests](../../issues/) to add documentation, links to tools, corrections or anything else are warmly accepted and gratefully appreciated!

Too busy to contribute directly, but still want to show your support? Please consider starring this project or tweeting about it!

## What is "globbing"?

The term "globbing", also referred to as "glob matching" or "filepath expansion", is a programming concept that describes the process of using wildcards, referred to as "glob patterns" or "globs", for matching file paths or other similar sets of strings.

Similar to [regular expressions](http://www.regular-expressions.info/), but much simpler and limited in scope, glob patterns are defined using a combination of special characters, or wildcards, alongside literal (non-matching) characters. For example, the glob pattern `*.txt` would match all files in a directory with a `.txt` file extension.

**Globbing syntax**

TODO: describe wildcards vs. extended globbing, and segue to following sections

* wildcards
* extended globbing

## Wildcards

The commonly supported characters supported across globbing implementations for basic wildcard matching are `*`, `?` and a simplified version of regex brackets for matching any of a given set of characters.

Although many different globbing implementations exist across a number of different languages and environments, the following table summarizes the most commonly supported basic globbing features.

| **Character** | **Description** | 
| --- | --- |
| `*` | Matches any character zero or more times, except for `/`. |
| `**` | Matches any character zero or more times, including `/`. |
| `?` | Matches any character one time |
| `[abc]` | Matches any of the specified characters (in this case, `a`, `b` or `c`) |

Special exceptions:

* `*` typically does not match dotfiles (file names starting with a `.`) unless explicitly enabled by the user [via options](#common-options)
* `?` also typically does not match the leading dot
* More than two stars in a glob path segment are typically interpreted as _a single star_ (e.g. `/***/` is the same as `/*/`)

**Implementations**

Globbing is typically enabled using third-party libraries. A notable exception, [bash](https://github.com/felixge/node-bash), provides built-in support for basic globbing.

TODO: add list of implementations

**Additional reading**

* [Wildcards - GNU/Linux Command-Line Tools Summary](http://tldp.org/LDP/GNU-Linux-Tools-Summary/html/x11655.htm#WILDCARDS)

## Extended globbing

In addition to [wildcard matching](#wildcards), extended globbing describes the addition of more advanced matching features, such as:

* brace expansion
* extglobs
* POSIX character classes
* regular expressions

TBC...

### brace expansion

In simple cases, brace expansion appears to work the same way as the logical `OR` operator. For example, `(a|b)` will achieve the same result as `{a,b}`.

Here are some powerful features unique to brace expansion (versus character classes):

* range expansion: `a{1..3}b/*.js` expands to: `['a1b/*.js', 'a2b/*.js', 'a3b/*.js']`
* nesting: `a{c,{d,e}}b/*.js` expands to: `['acb/*.js', 'adb/*.js', 'aeb/*.js']`

TBC...

Visit the [braces](https://github.com/micromatch/braces) library for more examples and information about brace expansion.

## extglobs

TBC...

As described by the bash man page:

| **pattern** | **regex equivalent** | **matches** | 
| --- | --- | --- |
| `?(a\|b\|c)` | `(a\|b\|c)?` | zero or one occurrence of the given patterns |
| `*(a\|b\|c)` | `(a\|b\|c)*` | zero or more occurrences of the given patterns |
| `+(a\|b\|c)` | `(a\|b\|c)+` | one or more occurrences of the given patterns |
| `@(a\|b\|c)` | `(a\|b\|c)` <sup>*</sup> | one of the given patterns |
| `!(a\|b\|c)` | N/A | anything except one of the given patterns |

<sup><strong>*</strong></sup> Note that `@` isn't a RegEx character.

Powered by [extglob](https://github.com/micromatch/extglob). Visit that library for the full range of options or to report extglob related issues.

See [extglob](https://github.com/micromatch/extglob) for more information about extended globs.

### POSIX character classes

POSIX character classes, or "bracket expressions", provide a way of defining regular expressions using something closer to plain english.

For example, the pattern `[[:alpha:][:digit:]]` would match `a1`, but not `aa`.

TBC...

See [expand-brackets](https://github.com/jonschlinkert/expand-brackets) for more information about extended bracket expressions.

### Regular expressions

This section describes matching using [regular expressions](http://www.regular-expressions.info/) _as it relates to globbing_.

#### regex character classes

When regex character classes are used in glob patterns, with the exception of brace expansion (`{a,b}`, `{1..5}`, etc), most of the special characters convert directly to regex, so you can expect them to follow the same rules and produce the same results as regex.

For example, given the list: `['a.js', 'b.js', 'c.js', 'd.js', 'E.js']`:

* `[ac].js`: matches both `a` and `c`, returning `['a.js', 'c.js']`
* `[b-d].js`: matches from `b` to `d`, returning `['b.js', 'c.js', 'd.js']`
* `[b-d].js`: matches from `b` to `d`, returning `['b.js', 'c.js', 'd.js']`
* `a/[A-Z].js`: matches and uppercase letter, returning `['a/E.md']`

However, there is

Learn about [regex character classes][character-classes].

#### regex groups

Given `['a.js', 'b.js', 'c.js', 'd.js', 'E.js']`:

* `(a|c).js`: would match either `a` or `c`, returning `['a.js', 'c.js']`
* `(b|d).js`: would match either `b` or `d`, returning `['b.js', 'd.js']`
* `(b|[A-Z]).js`: would match either `b` or an uppercase letter, returning `['b.js', 'E.js']`

As with regex, parentheses can be nested, so patterns like `((a|b)|c)/b` will work. But it might be easier to achieve your goal using brace expansion.

## Common options

The following options are commonly available on various globbing implementations.

| **Option name** | **Description** | 
| --- | --- |
| `extglob` | Enable extended globs. In addition to the traditional globs (using wildcards: `*`, `*`, `?` and `[...]`), extended globs add (almost) the expressive power of regular expressions, allowing the use of patterns like `foo/!(a | b)*` |
| `dotglob` | Allows files beginning with `.` to be included in matches. This option is automatically enabled if the glob pattern begins with a dot. Aliases: `dot` (supported by: [minimatch](https://github.com/isaacs/minimatch), [micromatch](https://github.com/micromatch/micromatch)) |
| `failglob` | report an error when no matches are found |
| `globignore` | allows you to specify patterns a glob should not match  Aliases: `ignore` (supported by: [minimatch](https://github.com/isaacs/minimatch), [micromatch](https://github.com/micromatch/micromatch)) |
| `globstar` | recursively match directory paths (enabled by default in [minimatch](https://github.com/isaacs/minimatch) and [micromatch](https://github.com/micromatch/micromatch), but not in [bash](https://github.com/felixge/node-bash)) |
| `nocaseglob` | perform case-insensitive pathname expansion |
| `nocasematch` | perform case-insensitive matching. Aliases: `nocase` (supported by: [minimatch](https://github.com/isaacs/minimatch), [micromatch](https://github.com/micromatch/micromatch)) |
| `nullglob` | when enabled, the pattern itself will be returned when no matches are found. Aliases: `nonull` (supported by: [minimatch](https://github.com/isaacs/minimatch), [micromatch](https://github.com/micromatch/micromatch)) |

## Risks

_WIP_

Like [regular expressions](http://www.regular-expressions.info/), glob patterns are a type of [regular language](https://en.wikipedia.org/wiki/Regular_language) that must first be interpreted by a computer program before any actual matching takes place. This introduces two areas of risk:

* interpreting patterns:
* performing matches:

**Interpreting patterns**

TODO (The process of parsing and compiling a glob pattern into a regular expression...)

**Performing matches**

TODO (The process of matching the compiled regular expression against a set of strings)

**Risks**

* Intentional denial-of-service (DoS) attacks from hackers
* Unintentional denial-of-service (DoS) attacks from agressively greedy wildcard patterns

## Related concepts

The following are (or include) similar conceps to globs:

* [regular expressions](http://www.regular-expressions.info/)
* tilde expansion
* [kleene star](https://en.wikipedia.org/wiki/Kleene_star)
* ksh

## Resources

**Documentation**

* bash (TODO: get link to docs and unit tests)

**Tools and software**

TODO (convert to table)

In alphabetical order:

* [bash-glob](https://github.com/micromatch/bash-glob) (JavaScript/node.js)
* [brace-expansion](https://github.com/juliangruber/brace-expansion) (JavaScript/node.js)
* [braces](https://github.com/micromatch/braces) (JavaScript/node.js)
* [expand-brackets](https://github.com/jonschlinkert/expand-brackets) (JavaScript/node.js)
* [glob](https://github.com/isaacs/node-glob) (JavaScript/node.js)
* [micromatch](https://github.com/micromatch/micromatch) (JavaScript/node.js)
* [nanomatch](https://github.com/micromatch/nanomatch) (JavaScript/node.js)
* [minimatch](https://github.com/isaacs/minimatch) (JavaScript/node.js)