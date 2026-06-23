# Installing JBang

Examples in [`../SKILL.md`](../SKILL.md) write `jbang …` for brevity. If `jbang`
is not on your `PATH`, install it once with one of the options below.

JBang needs **Java 8 minimum** (Java 17+ recommended), but you do **not** have to
install Java yourself — JBang downloads and installs an Eclipse Temurin JDK on
demand if no `java` is found. The zero-install one-liners below therefore need
*nothing* pre-installed.

After installing, running `jbang app setup` puts the JBang app scripts on `PATH`
and (for supported shells) adds `j!` as an alias for `jbang`.

## Universal (all platforms) — recommended

Bootstraps JBang (and a JDK if needed). The trailing argument after `-` is what
JBang runs, so `app setup` performs the install/PATH setup:

```bash
# Linux / macOS / Windows (bash) / AIX
curl -Ls https://sh.jbang.dev | bash -s - app setup
```

```powershell
# Windows PowerShell
iex "& { $(iwr -useb https://ps.jbang.dev) } app setup"
```

## Zero install — run without installing

Replace `app setup` with the script/alias you want to run; JBang runs it and
caches the build without permanently installing itself. Useful in CI or one-off
agent steps:

```bash
curl -Ls https://sh.jbang.dev | bash -s - <file-or-url-or-alias> [args...]
```

```powershell
iex "& { $(iwr -useb https://ps.jbang.dev) } <file-or-url-or-alias> [args...]"
```

## Wrapper — commit a repo-local JBang

Like the Maven/Gradle wrapper: pins JBang into a project so contributors and CI
run `./jbang` with nothing pre-installed.

```bash
jbang wrapper install      # creates ./jbang, ./jbang.cmd, etc.
echo ".jbang/" >> .gitignore   # .jbang/ is just a cache; don't commit it
```

## Package managers

| Platform        | Install                                                          | Upgrade                          |
|-----------------|-----------------------------------------------------------------|----------------------------------|
| SDKMAN! (Linux/macOS) | `sdk install jbang`                                       | `sdk upgrade jbang`              |
| asdf            | `asdf plugin-add jbang && asdf install jbang latest`            | `asdf install jbang latest`      |
| Homebrew (macOS)| `brew install jbangdev/tap/jbang`                              | `brew upgrade jbangdev/tap/jbang`|
| Chocolatey (Win)| `choco install jbang`                                          | `choco upgrade jbang`            |
| Scoop (Win)     | `scoop bucket add jbangdev https://github.com/jbangdev/scoop-bucket` then `scoop install jbang` | `scoop update jbang` |

SDKMAN! and asdf manage multiple JBang versions and also install Java, so they
are handy when you need a specific JDK alongside JBang.

## Manual install

Unzip the [latest binary release](https://github.com/jbangdev/jbang/releases/latest),
add the `jbang-<version>/bin` folder to your `$PATH`, and you are set.

## Verify

```bash
jbang version      # prints the installed version
jbang --help       # prints usage
```

By default JBang uses `~/.jbang` for config and build cache; override it with the
`JBANG_DIR` environment variable. More options and platform notes are in the
[installation docs](https://jbang.dev/documentation/jbang/latest/installation.html).
