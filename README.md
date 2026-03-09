# GHIN Skill

![Version](https://img.shields.io/github/v/tag/rgstephens/ghin-skill?label=version&sort=semver)

A [ClawHub](https://clawhub.ai/skills/golf-ghin) skill for the [GHIN](https://www.ghin.com) golf handicap system. Look up golfer handicap indexes, view recent scores, search for golfers by name, and track followed golfers.

## Installation

```bash
curl -fsSL https://raw.githubusercontent.com/rgstephens/ghin-skill/main/install.sh | bash
```

## Commands

| Command                       | Description                                    |
| ----------------------------- | ---------------------------------------------- |
| `ghin` / `ghin handicap`      | Show your handicap index and profile           |
| `ghin scores [ghin-number]`   | List recent scores                             |
| `ghin search --last <name>`   | Search for golfers by name                     |
| `ghin lookup <ghin-number>`   | Look up a specific golfer by GHIN number       |
| `ghin followed`               | List golfers you follow                        |

## Global Flags

| Flag              | Description                                        |
| ----------------- | -------------------------------------------------- |
| `-i, --id`        | GHIN number or email (or set `GHIN_ID` env var)    |
| `-p, --password`  | GHIN password (or set `GHIN_PASSWORD` env var)     |
| `--json`          | Output raw JSON                                    |

## Configuration

Credentials are required and can be provided via flags or environment variables:

| Flag         | Env Variable    | Description                       |
| ------------ | --------------- | --------------------------------- |
| `--id`       | `GHIN_ID`       | Your GHIN number or email address |
| `--password` | `GHIN_PASSWORD` | Your GHIN password                |

The recommended approach is to set them in your shell profile:

```sh
export GHIN_ID=123456
export GHIN_PASSWORD=yourpassword
```

## Examples

```bash
ghin
ghin scores --num-scores 5
ghin search --last Stephens --first Greg
ghin lookup 3315181 --state WA
ghin followed --scores --days 2
```

## Releases

See [GitHub Releases](https://github.com/rgstephens/ghin-skill/releases) for changelog and version history.
