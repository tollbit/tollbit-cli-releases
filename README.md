# Tollbit CLI

Command-line client for [Tollbit](https://tollbit.com) developer APIs: search the content index, inspect licensing rates, mint content tokens, and fetch licensed articles.

Binaries and installers for this distribution repo are published on **[Releases](https://github.com/tollbit/tollbit-cli-releases/releases)**.

## Install

#### Agent install prompt
**Agent?:** Install Tollbit from [github.com/tollbit/tollbit-cli-releases](https://github.com/tollbit/tollbit-cli-releases) using the install script or package that your environment needs (below), then run `tollbit guide --install <SKILLS_DIR>` to register the bundled skill.

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

### npm

```bash
npm install -g @tollbit/tollbit-cli
```

Then run `tollbit` from any terminal (the package downloads the native binary for your OS).

### Manual install

From **[GitHub Releases](https://github.com/tollbit/tollbit-cli-releases/releases)**, download the archive for your OS and CPU, verify SHA-256 against `tollbit_<version>_checksums.txt`, and put `tollbit` (or `tollbit.exe`) on your `PATH`.

## For AI coding agents

- Install `tollbit` so it is on **`PATH`** in every environment where you run shell commands (CI images, local sandboxes, agent runners).
- Run **`tollbit guide`** for orientation, billing cautions, and examples of where to point **`tollbit guide --install <SKILLS_DIR>`**.
- Configure auth once: **`tollbit login --api-key ... --user-agent ...`** (or `TOLLBIT_API_KEY` / `TOLLBIT_USER_AGENT`), then confirm with **`tollbit whoami`** before paid calls.
- Prefer **`--json`** when you need machine-readable output; treat **`fetch`** and **`token` + `content`** as **metered** operations—avoid duplicate calls for the same URL.

## What the CLI can do

| Command | Purpose |
|--------|---------|
| **`search`** | Query the Tollbit content index (filters, pagination). |
| **`rate`** | List licensing rates for one or more article URLs. |
| **`token`** | Mint a paid content token for a URL (requires price / currency / license type). |
| **`content`** | Fetch article body using an existing token. |
| **`fetch`** | One-shot: rate → token → content for a single URL. |
| **`whoami`** | Check saved credentials and run a lightweight auth probe. |
| **`login`** | Save API key and registered user-agent to the credentials file. |
| **`guide`** | Print the agent guide; optionally install bundled skill markdown. |
| **`connect`** | Interactive REPL with the same commands (optional for automation). |
| **`version`** | Print the CLI version string. |

Typical flows: **`search`** → pick a URL → **`rate`** → **`token`** + **`content`**, or **`fetch`** for a single-shot licensed read. Use **`--help`** on each command for flags (`--json`, `--debug`, `--config`, etc.).
