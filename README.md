# Obsidian to Bookstack Sync

`obsidian_to_bookstack`

[[toc]]

## Summary

Download remote files from Bookstack instance, that you may not have, along with sending any changes to the remote. After configuration, consider installing the companion plugin at <https://github.com/jaypyles/ObsidianToBookstackPlugin>

- All commands will now also be available to use with chapters. This means that a structure like this is now allowed:

```sh
- Shelf/
-- Book/
--- Chapter/
---- Note.md
--- Note.md
```

## Installing

Required:

```sh
pip install pipx
pipx ensurepath
```

Run with `pipx install .` inside the project to install to your machine.

## Config

You can use a `.env` file for the following secrets:

```ini
BOOKSTACK_BASE_URL="https://demo.bookstack.com"
BOOKSTACK_TOKEN_ID=<your_bookstack_api_token_id>
BOOKSTACK_TOKEN_SECRET=<your_bookstack_api_token_secret>
```

or you may choose to use whatever secret manager you want, like Doppler, so long as it sets those environment variables.
By default, the tool will look for `.env` in the root of the project, but can be set anywhere using the `--env` flag.

Configuration by default is located at `~/.config/obsidian_to_bookstack/conf.toml` but can also be set anywhere using the `--config` flag.

The format of the configuration file is as follows:

```toml
[wiki]
path = "/home/user/notes/"

[wiki.excluded]
shelves = ["private"]
```

Any shelves in `wiki.excluded.shelves` will not be uploaded to Bookstack.

### Configuring CLI Options

- **Verbose Mode**

    - `-v`, `--verbose`: This optional flag enables verbose logs, providing more detailed information during command execution.

- **Config File**

    - `-c`, `--config`: Specify the path to a configuration file (`*.toml`). If provided, the CLI will load settings from this file. If not specified, default settings will be used.

- **Environment File**

    - `-e`, `--env`: Specify the path to an environment file (`.env`).

Running commands after specifying config paths will continue with the last used path.

### Structure

The structure of the Obsidian Vault is pretty specific as it mirrors the Bookstack structure to be as in sync as possible.

```sh
- Shelves
  - Books
    - Chapter
      - Pages.md
    - Pages.md
```

## Commands

### `sync`

Calls:

- `local`
- `remote`

### `local`

Pulls down any missing local files

### `remote`

Pushes up any missing files in the remote

### `update`

Requires one of these flags to be specified:

- `--remote`
- `--local`

If `--remote` is specified, any files which have been updated locally will be changed in the remote and vice-versa for `--local`.

### `delete`

Requires one of:

- `--shelf`
- `--book`
- `--page`

Will `delete` Obsidian files and Bookstack files at the same time. Anything nested under a shelf or book will also be deleted.
Must be called in a path like structure.

Example:

```sh
- Shelf
  - Book
    - Chapter
      - Page.md
    - Page.md
```

The command would be called as:

to `delete` a page:

```sh
obsidian_to_bookstack delete Shelf/Book/Page --page
```

Optional configuration commands must be ran as:

```sh
obsidian_to_bookstack --verbose --config ~/.config/.. --env ~/.config/... <command>
```

## In Progress

- Statistical/table view of uploaded, downloaded, deleted, or updated objects
