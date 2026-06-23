# Source directives & script structure

JBang configures a script from `//`-prefixed lines inside the source file, so a
script stays a single self-contained file with no external build configuration.
Directives are read from `.java`, `.jsh`, `.kt`, `.groovy`, and `.md` sources.

## Making a file directly executable

Put this as the first line so the file can be run as `./hello.java` after
`chmod +x` — the line is a valid POSIX shebang and a no-op `///` comment to Java:

```java
///usr/bin/env jbang "$0" "$@" ; exit $?
```

You do **not** need it when you run `jbang hello.java` explicitly.

## Common directives

| Directive            | Purpose                                                                 |
|----------------------|-------------------------------------------------------------------------|
| `//DEPS g:a:v …`     | Maven dependencies (space- or line-separated, repeatable). `@pom` for BOMs. |
| `//JAVA 21+`         | Require/select a JDK version; JBang downloads it if missing.             |
| `//JAVA_OPTIONS …`   | JVM runtime options (e.g. `-Xmx512m`, `--enable-preview`).               |
| `//COMPILE_OPTIONS …`| Options passed to the compiler (e.g. `-parameters`).                     |
| `//SOURCES x.java`   | Pull in additional source files (globs allowed).                        |
| `//FILES a=b`        | Stage resource files relative to the script.                            |
| `//MAIN fqcn`        | Explicit main class when it isn't the file's own class.                 |
| `//REPOS id=url`     | Extra Maven repositories for dependency resolution.                     |
| `//GAV g:a:v`        | Group/artifact/version identity when exporting or publishing.           |
| `//KOTLIN 2.0.0`     | Kotlin version for `.kt` scripts (`//GROOVY` for Groovy).               |

Dependency versions accept `LATEST` and `RELEASE` placeholders, e.g.
`//DEPS org.example:lib:RELEASE`.

## Minimal examples

Bare script (no shebang needed when run via `jbang`):

```java
class hello {
    public static void main(String[] args) {
        System.out.println("Hello " + (args.length > 0 ? args[0] : "World"));
    }
}
```

With a dependency and a pinned JDK:

```java
//DEPS info.picocli:picocli:4.7.6
//JAVA 21+

import picocli.CommandLine;
import picocli.CommandLine.Command;

@Command(name = "greet", mixinStandardHelpOptions = true)
class greet implements Runnable {
    public static void main(String[] args) {
        new CommandLine(new greet()).execute(args);
    }
    public void run() { System.out.println("Hi!"); }
}
```

JShell snippet (`.jsh`) — runs top-level statements directly, no class needed:

```java
//DEPS org.apache.commons:commons-lang3:3.17.0
System.out.println(org.apache.commons.lang3.StringUtils.capitalize("jbang"));
```

## Running from anywhere

JBang accepts a local path, a remote URL, or a catalog alias as the target:

```bash
jbang hello.java                                   # local file
jbang https://github.com/user/repo/blob/main/x.java # remote (GitHub URL works directly)
jbang gist://<id>                                  # GitHub gist
jbang greet@user                                   # alias from a published catalog
```

See [`commands.md`](commands.md) for `init`, `edit`, `alias`, `app`, `export`,
and `--native`. Full reference:
<https://jbang.dev/documentation/jbang/latest/script-directives.html>.
