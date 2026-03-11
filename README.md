# wp-cli-local

An AI agent skill that runs [WP-CLI](https://wp-cli.org/) commands against [Local](https://localwp.com/) (by Flywheel) sites on macOS.

- **Auto-detects** the Local site from your current directory
- Falls back to `--site=<name>` when outside a site folder
- Uses Local's own PHP and MySQL binaries with the correct `php.ini`

## Installation

```bash
npx skills add https://github.com/soderlind/wp-cli-local --skill wp-cli-local -g
```

The `--all` flag installs the skill for all detected AI agents (GitHub Copilot, Claude Code, Cursor, Cline, etc.) without prompting:

```bash
npx skills add https://github.com/soderlind/wp-cli-local --skill wp-cli-local -g --all
```

Without `--all`, you'll be prompted to select which agents to install for.

## Prerequisites

- **macOS** (Apple Silicon or Intel)
- **[Local](https://localwp.com/)** installed with at least one site
- **[WP-CLI](https://wp-cli.org/)** installed and in `PATH` (e.g. `brew install wp-cli`)
- The target site must be **running** in Local

## How It Works

The wrapper script at `scripts/wp`:

1. Reads `~/Library/Application Support/Local/sites.json` to find all sites
2. Matches the current working directory against site paths (longest prefix match)
3. Resolves the correct PHP and MySQL binaries from Local's `lightning-services/`
4. Loads the site-specific `php.ini` (which contains the MySQL socket path)
5. Sets `MYSQL_UNIX_PORT` so `wp db` commands use the correct socket
6. Executes WP-CLI with the correct environment

## Usage

When an AI agent uses this skill, it calls the wrapper script automatically. For manual testing:

```bash
# From inside a Local site directory — auto-detects the site
bash scripts/wp plugin list

# Explicit site name
bash scripts/wp --site=my-site core version

# List all sites with running/halted status
bash scripts/wp --list
```

### Example output of `--list`

```
Local by Flywheel sites:

  ● plugins              running    /Users/you/Sites/plugins
  ○ my-site              halted     /Users/you/Sites/my-site
```

## Credit

Inspired by [local-wp-cli](https://github.com/aslamdoctor/local-wp-cli) skill, but rewritten from scratch with better site detection and MySQL socket support.

## License

MIT
