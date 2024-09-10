---
author: Steve Traugott
---

# CSWG Workshop: Making with 'make' and Makefiles

The 'make' command is available on most Unix-like systems -- find or
install it on your system, and follow along with the examples in this
document.

Make is a tool that helps you automate running shell commands.  It can
be used to:

- automatically run the right commands in the right order
- skip commands that don't need to be re-run
- manage prerequisites and dependencies
- manage a project or workflow
- automate a software build process
- build documentation from source files
- run tests and report results
- deploy software to target machines
- ...even build and maintain an entire computing infrastructure

---

## How do you use 'make'?

- create a Makefile in your project directory
- in the same directory, run the 'make' command:

```bash
$ ls
Makefile
$ cat Makefile
all: 
    echo "Hello, World!" > hello.txt
$ make
$ ls
Makefile hello.txt
$ cat hello.txt
Hello, World!
```

---

## What is a Makefile Rule?

A rule can be thought of as a function in a programming language.  It
takes a set of inputs, runs a set of shell commands, and produces a
set of outputs.  You run a rule by specifying its name on the 'make'
command line.

```make
# this is a rule named 'all'; it has no inputs
all: 
    # the output of 'make all' is the file 'hello.txt'
    echo "Hello, World!" > hello.txt
```

A Makefile rule is sometimes called a 'stanza'.

---

## Example Makefile

```make
# This is a rule named 'all'; it has no inputs.  You can run it by
# typing 'make all', or since it's the first rule in the Makefile, you
# can just type 'make'.
all: 
    # The output of 'make all' is the file 'hello.txt'.
    echo "Hello, World!" > hello.txt
```

Each time you run 'make all', it will re-create 'hello.txt'.

Makefiles use tabs for indentation -- in the example above, the line
with the 'echo' command must be indented with a tab character, not
spaces.

---

## Example Makefile with a prerequisite 

```make
# The 'all' rule depends on the 'hello' target.
all: hello

# The 'hello' rule makes the file 'hello.txt'.
hello: 
    echo "Hello, World!" > hello.txt
```

If you run 'make all', it will:
- run the 'hello' rule
- ...which depends on 'hello'
- ...which will run the command 'echo "Hello, World!" > hello.txt'

Each time you run 'make all', it will re-create 'hello.txt'.

---

## Example Makefile with a filename as a target

Listing a filename as a target in a Makefile is a common pattern.  If 
'make' sees a file with the same name as a target, it assumes that the
rule should only be run if the file is missing or out of date.

```make
# The 'all' rule depends on the file 'hello.txt'.
all: hello.txt

# The 'hello.txt' rule makes the file 'hello.txt'.
hello.txt:
    echo "Hello, World!" > hello.txt
```

The first time you run 'make', it will create 'hello.txt'.  If you run
'make' again, it will do nothing, because 'hello.txt' already exists.

---

## Example Makefile with a chain of prerequisites

```make
# The 'all' rule depends on the 'bye.txt' target.
all: bye.txt

# The 'bye.txt' rule depends on the 'hello.txt' target.  It will run
# the hello.txt rule first if hello.txt is missing or out of date, then
# run the command to create bye.txt.
bye.txt: hello.txt
    echo "Byebye!" > bye.txt

# The 'hello.txt' rule makes the file 'hello.txt'.
hello.txt:
    echo "Hello, World!" > hello.txt
```

The first time you run 'make', it will create 'hello.txt' and 'bye.txt'.
If you run 'make' again, it will do nothing, because 'hello.txt' and
'bye.txt' already exist.

But if you update the timestamp on 'hello.txt', 'make' will re-run the
'bye.txt' rule, because 'bye.txt' is out of date.

---

## Example Makefile with a variable

```make
# The 'all' rule depends on the 'greet.txt' target.
all: greet.txt

# The 'greeting' variable contains the contents that will be written to
# the 'greet.txt' file.
greeting = "Hi, World!"

# Variables are referenced with '$(variable_name)'.
greet.txt:
    echo $(greeting) > greet.txt
```

If you run 'make', it will create 'greet.txt' with the contents "Hi, World!".

---

## Example Makefile with an external variable

```make
all: greet.txt

# The 'greeting' variable is set to the value of the 'GREETING'
# environment variable.
greeting = $(GREETING)

greet.txt:
    echo $(greeting) > greet.txt
```

You can set the 'GREETING' environment variable before running 'make':

```bash
$ export GREETING="Bonjour, Monde!"
$ make
```

...or you can set it on the 'make' command line:

```bash
$ make GREETING="Bonjour, Monde!"
```

---

## What is 'make', really?

The Makefile language is a Turing-complete programming language,
meaning that it can in theory compute anything that can be computed by
a machine.

This makes it a powerful tool for automating complex workflows, up to
and including building and maintaining the computing infrastructure
for an entire organization.

Some useful search terms:
- "bootstrapping infrastructure makefile" 
- "devops makefile"

As an early example, the "Bootstrapping an Infrastructure" paper
describes how Makefiles were used to automate the process of building
and maintaining trading floors at a major international financial
institution.  Each machine, both desktop and server, ran 'make' at
bootup and periodically throughout the day to ensure that it was in
the correct state, installing, updating, and configuring operating
systems and applications as needed.
