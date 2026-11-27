# Linux File Operations, Search & Symbolic Links

## 1. What I Learned Today
After learning how to move around folders (`cd`), I focused on actually working with files—reading what is inside them, searching for specific text, comparing changes, and organizing files and shortcuts from the terminal.

---

## 2. Reading, Searching & Comparing Files
Instead of opening a text editor with a mouse, Linux gives fast command-line tools to inspect file contents:

* **`cat` (Concatenate):** Prints the entire contents of a file straight onto the terminal screen. It works with both relative paths (`cat notes.txt`) and absolute paths (`cat /var/log/syslog`).
* *Operational Note:* `cat` works best on small files; for multi-gigabyte production logs, use `less`, `head`, or `tail -f` so you don't flood your terminal or stall an SSH session.
* **`grep`:** Searches inside a file for a specific word or pattern and prints only the matching lines—like finding a needle in a haystack.
  * *Example:* `grep "Failed password" /var/log/auth.log`
* **`diff`:** Compares two files line-by-line and displays only what is different between them. Helpful for spotting edits between an old backup file and a new file.

---

## 3. Managing Files & Folders
* **`ls` (List):** Shows what is inside the current folder. Adding the `-a` flag (`ls -a`) reveals **hidden files**—any file or folder whose name starts with a dot (like `.bashrc`).
* **`touch`:** Creates a brand-new, empty file (`touch draft.txt`).
* **`mkdir` (Make Directory):** Creates a new folder (`mkdir projects`).
* **`cp` (Copy):** Makes a duplicate of a file in another location (`cp file.txt backup/file.txt`).
* **`mv` (Move):** Moves a file to a different folder—or renames it if kept in the same folder (`mv oldname.txt newname.txt`).
* **`rm` (Remove):** Permanently deletes a file (`rm temp.txt`). Unlike Windows or macOS, there is no Recycle Bin in the terminal, so deleted files are gone immediately.

---

## 4. Symbolic Links (`ln -s`)
A **symbolic link** (or symlink) works like a shortcut on a desktop—it is a special pointer file that redirects to another file or folder somewhere else on the system.

* **Command:** `ln -s /path/to/original_file shortcut_name`
* **Why it matters:** Programs and Linux itself use symlinks to point to active configuration files or tools without having to duplicate the actual data.

---

## 5. Security & Operational Takeaways

* **SOC / Incident Triage:**
  * **Spotting Persistence with `ls -a`:** Attackers frequently hide unauthorized SSH keys (`~/.ssh/authorized_keys`), staging directories, or persistence scripts inside dot-prefixed paths—never rely on a plain `ls` during host triage.
  * **Configuration Drift & Integrity (`diff`):** Comparing live critical files (`/etc/passwd`, `/etc/ssh/sshd_config`, or crontabs) against known-good baselines with `diff` is a fast manual way to detect unauthorized changes during both incident response and GRC compliance audits.
  * **Safe Log Inspection:** Pairing `grep` with targeted log paths (`/var/log/auth.log`) isolates brute-force or anomalous activity quickly without dumping entire multi-gigabyte files to the console.
* **Offensive / AppSec Considerations:**
  * **System & Operational Hygiene:**
    * **Broken Symlinks (`ln -s`):** Because a symlink only stores a path pointer rather than a copy of the data, moving or deleting the original file breaks the shortcut. Using `ls -l` reveals where a symlink actually points (`->`) before modifying or deleting it.
    * **Irreversible Deletion (`rm`):** Terminal deletion bypasses any GUI trash bin—double-checking paths with `ls` before running `rm` prevents accidental loss of logs, configs, or scripts.
