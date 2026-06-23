---
name: jbang
description: >
  Run, create, and share Java (and Kotlin/Groovy/JShell/Markdown) scripts with
  zero project setup using the JBang CLI. Use this skill whenever the user wants
  to run a single .java/.jsh/.kt/.groovy file directly, declare Maven
  dependencies inline with //DEPS, scaffold a script from a template
  (jbang init), open a script with full IDE support (jbang edit), turn a script
  into a CLI command (jbang app install), pin or pick a JDK with //JAVA, run a
  script straight from a GitHub URL or alias/catalog, export a script to a
  Maven/Gradle project, build a native image, or needs JBang installed in the
  first place with no prerequisites. Covers installation (curl/bash, PowerShell,
  package managers, wrapper), the //-style source directives, and the everyday
  commands.
metadata:
  source: https://github.com/jbangdev/jbang
  agent-generic: "true"
---

# Running Java instantly with JBang

Agent-generic guide (works with any coding assistant) for using the
[JBang](https://github.com/jbangdev/jbang) CLI to run and manage Java scripts
without a build file or project setup.

For building and contributing to JBang itself, see [`AGENTS.md`](../../AGENTS.md).
Full user docs live at <https://jbang.dev/documentation>.

## What JBang does

JBang runs `.java`, `.jsh`, `.kt`, `.groovy`, and `.md` source files directly —
no `pom.xml`, no `build.gradle`, no `main` boilerplate required. It resolves
Maven dependencies declared inline, downloads a JDK if none is present, caches
the build, and runs the result.

```bash
jbang <file-or-url-or-alias> [args...]
```

If `jbang` is not on `PATH`, install it first — see
[`references/installing-jbang.md`](references/installing-jbang.md). The
zero-prerequisite path (no JDK, no JBang pre-installed) is:

```bash
# Linux / macOS / Windows (bash)
curl -Ls https://sh.jbang.dev | bash -s - <file-or-url-or-alias> [args...]
# Windows PowerShell
iex "& { $(iwr -useb https://ps.jbang.dev) } <file-or-url-or-alias> [args...]"
```

## Quickest path

```bash
# Install JBang (one-time) and run the first script
curl -Ls https://sh.jbang.dev | bash -s - app setup
jbang init hello.java        # scaffold
jbang hello.java World       # run, passes "World" as an arg
```

A bare class with a `main` method is enough — JBang does not need the
`///usr/bin/env jbang` shebang to run a file you pass to it explicitly. The
shebang only matters when you want the file itself to be directly executable
(`chmod +x hello.java && ./hello.java`).

## Inline source directives

Configuration lives in the source file as `//`-prefixed lines, so a script stays
a single self-contained file. The common ones:

```java
//DEPS info.picocli:picocli:4.7.6 org.slf4j:slf4j-simple:2.0.17
//JAVA 21+
//SOURCES Utils.java
//FILES logback.xml=src/logback.xml
//JAVA_OPTIONS -Xmx512m
//COMPILE_OPTIONS -parameters
//MAIN com.example.Other
```

`//DEPS` takes Maven `group:artifact:version` coordinates (space- or
line-separated, repeatable). `//JAVA 21+` makes JBang download/select a matching
JDK. Full directive reference:
[`references/running-scripts.md`](references/running-scripts.md).

## Everyday commands

| Goal                              | Command                                            |
|-----------------------------------|----------------------------------------------------|
| Run a local file / URL / alias    | `jbang hello.java` · `jbang https://…/x.java`      |
| Scaffold from a template          | `jbang init --template=cli app.java`               |
| Open with IDE support             | `jbang edit --open=code hello.java`                |
| Add a dependency interactively    | `jbang init` then edit `//DEPS`                    |
| Run with an explicit JDK          | `jbang --java 21 hello.java`                        |
| Install as a system command       | `jbang app install --name greet hello.java`        |
| Make a named alias                | `jbang alias add --name greet hello.java`          |
| Export to a Maven/Gradle project  | `jbang export maven hello.java`                    |
| Build a native binary             | `jbang --native hello.java`                         |
| Commit a repo-local jbang         | `jbang wrapper install`                            |

Per-command detail and flags:
[`references/commands.md`](references/commands.md).

## Picking what to do

- **"Just run this Java file"** → `jbang <file>`. If it needs libraries, add a
  `//DEPS` line rather than creating a project.
- **"It needs Java 21 / a specific JDK"** → add `//JAVA 21+` to the source, or
  pass `--java 21`. JBang installs the JDK if missing; never hand-install a JDK
  just to run a script.
- **"Share / reuse this"** → `jbang app install` (becomes a command on PATH),
  `jbang alias add` (named shortcut, publishable in a catalog), or just point
  people at the GitHub URL — `jbang https://github.com/…` runs it directly.
- **"Make it a real project"** → `jbang export maven|gradle <file>` emits a
  conventional project; don't reconstruct one by hand.
- **"No tools installed at all"** → use the `curl … | bash -s -` /
  `iex …` zero-install one-liners above; they bootstrap JDK + JBang on demand.
