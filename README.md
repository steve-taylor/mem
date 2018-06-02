# mem - Maven Environment Manager

## Problem

You build software for multiple companies, or perhaps just one company, but
you also have some side projects. You do all this on the same machine. Each
company has their own Maven artifact server, each with its own settings.xml
file and potentially other settings and company-specific build artifacts, all
wanting to be stored in the same `~/.m2` directory. You know you can use
Docker or Vagrant to keep things isolated, but feel they are extreme
solutions to this problem. You really just need a simple way to switch between
Maven environments.

## Solution

`mem` (Maven Environment Manager) is a simple command-line tool to create
and switch between Maven environments. It works by converting your `~/.m2`
directory into a symbolic link which points to your current environment,
located at `~/.m2.<environment>`, e.g. `.m2.foobar` for the *foobar*
environment.

## System requirements

`mem` is known to work in the following environments:

* Linux distributions with bash
* macOS / OS X
* Windows Subsystem for Linux

`mem` doesn't work with Git Bash as it cannot create symbolic links.

## Usage

### Initialize

To start managing your Maven environments with `mem`:

```sh
mem init
```

This will rename your `~/.m2` directory to `~/.m2.default` and create a
symbolic link pointing `~/.m2` to `~/.m2.default`. This effectively creates
the first environment, called *default*, and switches to that environment.

### Create an environment

To create an environment and switch to it:

```sh
mem create <environment>
```

For example, to create a new environment called *work*:

```sh
mem create work
```

### Switch to an environment

To switch to an environment:

```sh
mem use <environment>
```

For example, to switch to an environment called *side-projects*:

```sh
mem use side-projects
```

### List environments

To list all your Maven environments:

```sh
mem
```

Example output:

```
Maven environments:

 * default
 * side-projects [active]
 * work
```

### Delete an environment

To delete an environment:

```sh
mem delete <environment>
```

For example, to delete the environment called *side-projects*:

```sh
mem delete side-projects
```

#### Note:

`mem` will not allow you to delete the currently active environment, because
there must always be exactly one active environment and `mem` isn't smart
enough to choose the environment which should become active after the active
environment is deleted.

To delete the currently active environment, you must first switch to another
environment.

## Technical details

`mem` is a bash script with no dependencies on things like `sed` and `awk`,
which may not be found in all bash environments.
