---
name: golf-ghin
description: Look up golf handicap information from GHIN (Golf Handicap and Information Network). Use when the user asks about a golfer's handicap index, GHIN number, recent scores, score differentials, or wants to search for a golfer by name.
metadata:
  {
    "nanobot":
      {
        "emoji": "⛳",
        "requires": { "bins": ["ghin"] },
        "install":
          [
            {
              "id": "curl",
              "kind": "script",
              "label": "Install ghin CLI",
              "url": "https://raw.githubusercontent.com/rgstephens/ghin-skill/main/install.sh",
            },
          ],
      },
  }
---

<!-- Version: 0.2.2 -->

# GHIN Golf Handicap

Look up golf handicap information via the [GHIN](https://www.ghin.com) API using the `ghin` CLI tool.

## Installation

```bash
curl -fsSL https://raw.githubusercontent.com/rgstephens/ghin-skill/main/install.sh | bash
```

Pin a specific version:

```bash
VERSION=v1.2.3 bash <(curl -fsSL https://raw.githubusercontent.com/rgstephens/ghin-skill/main/install.sh)
```

By default the binary is installed to `/usr/local/bin/ghin`. Set `INSTALL_DIR` to override.

---

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

---

## Commands

### `ghin` / `ghin handicap` — Show your handicap index and profile

```bash
ghin
ghin handicap
```

Output includes: Name, GHIN Number, Handicap Index, Club, State, Status, Revision Date, Low HI.

### `scores` — List recent scores

```bash
ghin scores [ghin-number]
```

```bash
ghin scores
ghin scores --num-scores 5
ghin scores 3315181 --num-scores 10
```

| Flag               | Default | Description                  |
| ------------------ | ------- | ---------------------------- |
| `-n, --num-scores` | `10`    | Number of scores to retrieve |
| `--offset`         | `1`     | Offset to start from         |

### `search` — Search for golfers by name

```bash
ghin search --last <name> [--first <name>]
```

```bash
ghin search --last Palmer --first Arnold
ghin search --last Smith --per-page 50
```

| Flag          | Default | Description          |
| ------------- | ------- | -------------------- |
| `-f, --first` | —       | First name           |
| `-l, --last`  | —       | Last name (required) |
| `--page`      | `1`     | Page number          |
| `--per-page`  | `25`    | Results per page     |

### `lookup` — Look up a specific golfer by GHIN number

```bash
ghin lookup <ghin-number>
```

```bash
ghin lookup 3315181
ghin lookup 3315181 --state WA
```

| Flag          | Description                              |
| ------------- | ---------------------------------------- |
| `-s, --state` | Filter by state abbreviation (e.g. `WA`) |

### `followed` — List golfers you follow

```bash
ghin followed
ghin followed --scores
ghin followed --scores --days 2
```

| Flag       | Default | Description                                 |
| ---------- | ------- | ------------------------------------------- |
| `--scores` | —       | Show recent scores for all followed golfers |
| `--days`   | `1`     | Number of days back to look for scores      |

### Version

```bash
ghin --version
```

---

## Global Flags

These flags work with every command:

| Flag             | Description                                     |
| ---------------- | ----------------------------------------------- |
| `-i, --id`       | GHIN number or email (or set `GHIN_ID` env var) |
| `-p, --password` | GHIN password (or set `GHIN_PASSWORD` env var)  |
| `--json`         | Output raw JSON response from the API           |
| `-h, --help`     | Show help                                       |

---

## JSON Output

Every command supports `--json` to print the raw API response instead of formatted output. Status messages are written to stderr so they don't interfere with piping.

```bash
# Pipe to jq for filtering
ghin --json | jq '.golfer.handicap_index'
ghin scores --json | jq '.scores[] | {date: .played_at, differential}'
ghin search --last Stephens --json | jq '.golfers[] | {ghin, name: (.first_name + " " + .last_name), hi: .handicap_index}'
ghin lookup 3315181 --json | jq '.golfers[0]'
ghin followed --json | jq '.followed_golfers[] | select(.handicap_index < 5)'
```
