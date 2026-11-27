# Querying System Documentation & Help

> **Core Takeaway:** In cybersecurity and system administration, you don't memorize every command flag—you learn how to query offline manuals, built-in help, and keyword indexes in seconds without leaving the terminal.

---

## 1. Reading Manuals (`man`)

Most standard Linux utilities ship with offline manual pages (`man` pages) that document their syntax, flags, and exit codes.

```bash
man ls
man grep
```

### Essential Navigation Keys Inside `man`
* **`Space` / `Page Down`:** Scroll down one screen
* **`b` / `Page Up`:** Scroll up one screen
* **`q`:** Quit the manual and return to the shell prompt

### Manual Sections
Manual pages are grouped into numbered sections (for example, Section `1` covers user commands and Section `5` covers configuration file formats). If a command and a config file share a name, specify the section number:

```bash
man 5 passwd
```

---

## 2. Searching Inside a Manual (`/` and `n`)

Long manuals (like `man bash` or `man curl`) can span hundreds of lines. Instead of scrolling line-by-line, search directly inside the open pager (for example, looking up `-r` or `recursive` inside `man grep`):

1. Press `/` followed by your search term (e.g., `/recursive`) and press **Enter**.
2. Press `n` to jump forward to the next match.
3. Press `N` (`Shift` + `n`) to jump backward to the previous match.

---

## 3. Searching *For* Manuals (`man -k` / `apropos`)

When you know **what** you want to accomplish (e.g., inspecting network sockets or changing ownership) but don't know the command name, search across all manual descriptions by keyword:

```bash
man -k socket
apropos ownership
```

Both commands scan short one-line descriptions of installed manual pages and list matching utilities along with their section numbers.

---

## 4. Helpful Programs (`--help` / `-h`)

Custom scripts, third-party tools, and standalone binaries often do not have a `man` page. Most provide a built-in usage summary when invoked with `--help` or `-h`:

```bash
/path/to/program --help
```

### Reading `--help` Output Cleanly
* **Handle long output:** Pipe help text into `less` or `grep` so the top options don't scroll off screen:
  ```bash
  curl --help | less
  curl --help | grep -i "header"
  ```
* **Watch out for placeholders:** Uppercase words in usage strings (like `--timeout SECONDS` or `-f FILE`) represent **input placeholders** (a number or a file path), not literal words you type verbatim.

---

## 5. Help for Shell Built-ins (`help`)

Commands like `cd`, `export`, `echo`, and `read` are **shell built-ins**—they are baked directly into Bash rather than existing as standalone binaries on disk.

Running `man cd` often opens the generic built-ins page rather than a dedicated manual. Instead, use the shell's native `help` command:

```bash
help cd
help export
```

---

## Quick Reference Cheat Sheet

| Goal | Command / Shortcut | Example |
|---|---|---|
| Open a tool's manual | `man <command>` | `man find` |
| Search inside an open `man` page | `/<term>` then `n` / `N` | `/hidden` |
| Find commands by keyword | `man -k <keyword>` or `apropos <keyword>` | `man -k permission` |
| Query a program's quick usage | `<program> --help` (or `-h`) | `nmap --help` |
| Look up a shell built-in | `help <builtin>` | `help cd` |
