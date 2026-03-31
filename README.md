# mo-git-history

single file history viewer. use `mo`.

`mo-git-history` opens one Markdown file and every committed revision of that file
in `mo`, ordered from newest to oldest. The current working tree file is opened
first, followed by historical snapshots materialized into a temporary directory.

## Movie

https://github.com/user-attachments/assets/829caa5a-2ed8-4348-b855-33f7774ca2b0

## Installation

`mo-git-history` is a single Bash script.  
Just place it in a directory included in your `$PATH` and make it executable.

For example, you can install it to `/usr/local/bin` like this:

```bash
sudo curl -o /usr/local/bin/mo-git-history \
  https://raw.githubusercontent.com/smeghead/mo-git-history/refs/heads/main/mo-git-history
sudo chmod +x /usr/local/bin/mo-git-history
```

> [!NOTE]
> Adjust the install path depending on your environment.

## Usage

```bash
./mo-git-history path/to/spec.md
```

The wrapper:

- accepts exactly one file argument
- uses `git log` for the current path only
- does not follow renames
- starts `mo` with `--foreground`
- picks a random available port for each run so existing `mo` sessions do not mix with the history view
- removes its temporary files when `mo` exits or the wrapper receives `INT`, `TERM`, or `HUP`

The file order passed to `mo` is:

1. current working tree file
2. latest committed revision for that path
3. older committed revisions in descending commit order

## Constraints

- the target file must exist in the working tree
- the target file must be inside a git repository
- the target file must have at least one committed revision
- very long histories will create many temporary files before `mo` starts





## mo install

```bash
curl -LO https://github.com/k1LoW/mo/releases/download/v0.23.1/mo_0.23.1-1_amd64.deb
sudo apt install ./mo_0.23.1-1_amd64.deb
```
