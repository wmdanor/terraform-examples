# Terraform Examples

This repository is dedicated to Terraform Examples.

In most if not all examples, GNU Make will be used
to help with automation organisation.

* * [AWS Examples](./aws)

## FAQ

### What is Make

Make is a utility that automatically determines which pieces
of a large program need to be recompiled, and issues commands
to recompile them.

It is commonly used for C programs, but you can use make 
with any programming language whose compiler can be run 
with a shell command. Indeed, make is not limited to programs.
You can use it to describe any task where some files must 
be updated automatically from others whenever the others change.

Here, though, we will most use `.PHONY` targets.
In context of Make, `.PHONY` target is a target that will
always run, if it is deemed necessary for completing the
goal by Make. Let's say you have `.PHONY` `build` target,
and then type in `make build` to your shell. Then this
target will be executed.

I would recommend reading the following manual about make,
as this is very useful tool for any developer, and the
biggest advantage, in my opinion, is the fact that it
comes preinstalled almost everywhere and used as a
standard build tool in many places.

https://www.gnu.org/software/make/manual/html_node/index.html#SEC_Contents

The reason I decided to use Make in this example is 

### Breakdown of wildcard target

Let's take a look at simplified fragment of one of the wildcard targets:

```make
functions := $(shell ls functions)

build: $(functions:%=build/%)
build/%: FUNCTION=$*
build/%:
	@echo "build $(FUNCTION)"
```

First of all:

*  `build/%` - is a wildcard target, `build/` is what the target must start with,
    while `%` matches any nonempty substring. I do recommend reading manual here to
    understand more https://www.gnu.org/software/make/manual/html_node/Pattern-Rules.html.
    For example it can match the following goals: 
    * `build/foo`
    * `build/function/foo`
* `$*` is an **automatic variable**: it represents the stem of the target matched by `%`.
  So if you're building `build/function/foo`, then `$*` is `function/foo`.
* `$(function:%=build/%)` - is a string substituion.
  https://www.gnu.org/software/make/manual/html_node/Substitution-Refs.html.

Now let's break everything down line by line:

1. `functions := $(shell ls functions)` - finds directories inside lambda folder.
2. `build: $(functions:%=build/%)` - defines main `build` target and makes it dependent on
    building all of the lambdas.
    * If `functions` is `foo bar`, then it would be
      expanded to `build: build/foo build/bar`.
3. `build/%: FUNCTION=$*` - assings `DIR` variable to lambda path.
    * `$*` is an **automatic variable**: it represents the stem of the target matched by `%`.
      So if you're building `build/foo`, then `$*` is `foo`.
4. `build/%` and subsequent `@echo "build $(FUNCTION)"` - will print out:
    * `build foo` for funcion in folder `functions/foo`
    * `build bar` for funcion in folder `functions/bar`
