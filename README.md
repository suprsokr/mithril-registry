# Mithril Mod Registry

Community registry of mods for [Mithril](https://github.com/suprsokr/mithril), the WoW 3.3.5a modding framework.

## Browsing Mods

Use the Mithril CLI to search and install mods:

```bash
mithril mod registry search "spell"
mithril mod registry info custom-spells
mithril mod registry install custom-spells
```

Or browse the [`mods/`](mods/) directory directly.

Installing a mod clones its git repository so you have the full source. Build it locally with `mithril mod build`.

## Registering Your Mod

1. Push your mod to a git repository (e.g. GitHub)
2. Fork this repository
3. Create a JSON file in `mods/` named `<your-mod-name>.json`
4. Submit a pull request

### Mod JSON Schema

```json
{
  "name": "my-awesome-mod",
  "description": "A short description of what the mod does",
  "author": "your-github-username",
  "repo": "https://github.com/your-username/my-awesome-mod",
  "tags": ["spells", "pvp", "balance"],
  "version": "1.0.0",
  "mod_types": ["dbc", "sql", "addon"]
}
```

### Fields

| Field | Required | Description |
|---|---|---|
| `name` | Yes | Unique mod name (lowercase, hyphens, must match filename) |
| `description` | Yes | Short description (one line) |
| `author` | Yes | GitHub username of the mod author |
| `repo` | Yes | URL to the mod's git repository |
| `tags` | No | Searchable tags (e.g., `spells`, `items`, `ui`, `pvp`, `pve`, `balance`, `fun`) |
| `version` | No | Latest version string |
| `mod_types` | No | Types of content: `dbc`, `addon`, `sql`, `core`, `binary-patch` |
| `releases` | No | URLs to pre-built artifacts for non-mithril users (optional) |

### Optional: Pre-Built Release Artifacts

If you want to support users who don't use mithril and prefer to manually install mod files, you can upload pre-built artifacts to a GitHub release and reference them in your registry entry:

```json
{
  "releases": {
    "latest": "https://github.com/your-username/my-awesome-mod/releases/latest"
  }
}
```

The release should contain:

- **`client.zip`** — Client-side files:
  ```
  client/
  ├── Data/
  │   └── patch-A.MPQ            # DBC patch
  ├── Data/enUS/
  │   └── patch-enUS-A.MPQ       # Addon patch
  └── binary-patches/
      └── *.json                  # Binary patch descriptors
  ```

- **`server.zip`** — Server-side files:
  ```
  server/
  ├── sql/
  │   └── world/
  │       └── 001_changes.sql     # SQL migrations
  ├── core-patches/
  │   └── 001_feature.patch       # TrinityCore patches
  └── dbc/
      └── Spell.dbc               # DBC files for the server
  ```

**Never include `Wow.exe`** (patched or otherwise) in release artifacts.

Mithril users don't need release artifacts — `mithril mod registry install` clones the source repo and builds locally.

## Directory Structure

```
mithril-registry/
├── README.md
├── schema.json            # JSON Schema for mod files
└── mods/
    ├── fly-in-azeroth.json
    └── ...
```

## Guidelines

- Mod names must be unique, lowercase, and use hyphens (no spaces or underscores)
- One JSON file per mod
- Keep descriptions concise — save details for your mod's own README
- Tag your mod appropriately so others can find it
- Test your mod before registering
- Include a license in your mod's repository
