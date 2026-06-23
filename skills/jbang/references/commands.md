# Everyday JBang commands

Run `jbang <command> --help` for the full flag list of any command. Global
options apply to every command:

| Option            | Purpose                                              |
|-------------------|------------------------------------------------------|
| `--verbose` / `--quiet` | More / less logging on stderr.                 |
| `--java <ver>`    | Run with a specific JDK (downloads it if missing).   |
| `--native`        | Build & run as a GraalVM native image.               |
| `--fresh`         | Ignore caches and re-resolve/rebuild.                |
| `--offline` / `-o`| Resolve only from local caches; no network.          |
| `--insecure`      | Allow untrusted TLS when fetching remote scripts.    |

## run (default)

Running is the default verb, so `jbang foo.java` == `jbang run foo.java`. Target
may be a file, URL, gist, or alias. Everything after the target is passed to the
script:

```bash
jbang run --java 21 hello.java Alice
jbang hello.java Alice            # same thing
```

## init — scaffold a script

```bash
jbang init hello.java                     # bare script
jbang init --template=cli app.java        # picocli CLI template
jbang init --template=agent app.java      # other built-in templates
jbang template list                       # see available templates
```

## edit — open with IDE support

Generates a throwaway project with resolved dependencies so any IDE gives full
completion, then opens it:

```bash
jbang edit --open=code hello.java         # VS Code; also idea, gitpod, etc.
jbang edit --live --open=code hello.java  # re-sync deps as you edit
```

## alias & catalog — named shortcuts

```bash
jbang alias add --name greet hello.java   # local alias -> jbang greet
jbang alias list
jbang catalog add --name mine https://github.com/user/repo  # consume a remote catalog
```

Aliases live in `jbang-catalog.json`; publishing that file in a repo lets others
run `jbang greet@user`.

## app — install as a system command

```bash
jbang app install --name greet hello.java # adds `greet` to PATH
jbang app install picocli@jbangdev        # install straight from a catalog alias
jbang app list
jbang app uninstall greet
jbang app setup                           # (re)configure PATH / j! alias
```

## export — turn a script into a project or artifact

```bash
jbang export maven hello.java             # emit a Maven project
jbang export gradle hello.java            # emit a Gradle project
jbang export portable hello.java          # self-contained jar + deps
jbang export native hello.java            # native binary
```

## wrapper — repo-local jbang

```bash
jbang wrapper install                     # create ./jbang for this directory
```

## Other useful verbs

| Command            | Purpose                                              |
|--------------------|------------------------------------------------------|
| `jbang version`    | Print version (`--update` to self-update).           |
| `jbang info tools` | Show resolved deps, JDK, and classpath for a script. |
| `jbang cache clear`| Clear build/dependency caches.                       |
| `jbang jdk list`   | List/install/manage JDKs JBang knows about.          |
| `jbang completion` | Emit shell completion script.                        |

Full command reference:
<https://jbang.dev/documentation/jbang/latest/cli/jbang.html>.
