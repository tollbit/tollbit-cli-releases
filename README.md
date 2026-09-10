# Tollbit CLI

Command-line client for [Tollbit](https://tollbit.com): discover and invoke agent functions staked on publisher properties.

**Current version:** v0.1.2

Binaries are published on **[Releases](https://github.com/tollbit/tollbit-cli-releases/releases)**.

## Install

**Agent?:** Install with the script for your OS below, then run `tollbit guide --install <SKILLS_DIR>` to register the bundled skill.

### macOS and Linux

```bash
curl -fsSL "https://raw.githubusercontent.com/tollbit/tollbit-cli-releases/main/scripts/install.sh" | bash
```

Pin a version or choose an install directory:

```bash
curl -fsSL "https://raw.githubusercontent.com/tollbit/tollbit-cli-releases/main/scripts/install.sh" | bash -s -- --version vX.Y.Z
curl -fsSL "https://raw.githubusercontent.com/tollbit/tollbit-cli-releases/main/scripts/install.sh" | bash -s -- --install-dir "$HOME/bin" --force
```

By default the binary goes under `~/.local/bin`; the installer can add that directory to your shell `PATH`.

### Windows (PowerShell)

```powershell
irm "https://raw.githubusercontent.com/tollbit/tollbit-cli-releases/main/scripts/install.ps1" | iex
```

Pin a version or skip `PATH` changes (useful in CI):

```powershell
irm "https://raw.githubusercontent.com/tollbit/tollbit-cli-releases/main/scripts/install.ps1" | iex
Install-Tollbit -Version vX.Y.Z -Force
Install-Tollbit -NoModifyPath -PrintPathInstructions
```

### Manual install

From **[GitHub Releases](https://github.com/tollbit/tollbit-cli-releases/releases)**, download the archive for your OS and CPU, verify SHA-256 against `tollbit_<version>_checksums.txt`, and put `tollbit` (or `tollbit.exe`) on your `PATH`.

### Update

Re-run the installer for your platform (add `--force` / `-Force` to replace an existing binary). Pin with `--version` / `-Version` when you need a specific release.

## Quick start

1. Set an agent name and confirm identity:

```bash
tollbit auth set --name my-agent
tollbit auth status
```

2. When a function needs linked user/org authorization:

```bash
tollbit auth login
tollbit auth status
```

3. Discover and invoke functions on a property:

```bash
tollbit connect example.com help
tollbit connect example.com list-fns "convert html to markdown"
tollbit connect example.com markdowner --help
tollbit connect example.com markdowner convert --url=https://example.com
```

**Invocations are metered.** Checking help, listing functions, and `auth status` are free; each successful function invoke bills per the function's pricing. Check `--help` on a function before you call it, and avoid repeating the same paid call.

For AI coding agents: keep `tollbit` on `PATH`, run `tollbit guide` (or `tollbit guide --install <SKILLS_DIR>`), prefer `--json` when you need machine-readable output, and treat connect invocations as metered.

## Commands

| Command | Purpose |
|--------|---------|
| `auth` | Agent identity and optional user/org authorization (`set`, `login`, `complete`, `status`, `logout`) |
| `connect PROPERTY` | Browse and invoke agent functions on a property (`help`, `list-fns`, invoke) |
| `list-fns "query"` | Search functions (`--property` required) |
| `guide` | Print the agent guide; `--install <SKILLS_DIR>` writes the bundled skill |
| `version` | Print the CLI version |
| `dev functions` | Create and manage provider agent functions (see below) |

Use `tollbit <command> --help` for flags. Inside an interactive `connect` session, type commands without the `tollbit connect <property>` prefix.

## Publish a function (providers)

Requires linked authorization (`tollbit auth login` if `auth status` does not show authorization as present).

```bash
tollbit auth status

tollbit dev functions draft \
  --function-slug example-search \
  --openapi-spec-url https://example.com/openapi.yaml

tollbit dev functions submit example-search
tollbit dev functions get example-search

# After the function is verified:
tollbit dev functions stake example-search --property example.com
```

Use hostname-only properties (no protocol or path). `unstake` removes publication from a property; `withdraw` returns a function to draft for revision.
