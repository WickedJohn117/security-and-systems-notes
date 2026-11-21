# Linux File Operations, Search & Symbolic Links

## 1. What I Learned Today
After learning how to move around folders (`cd`), I focused on actually working with files—reading what is inside them, searching for specific text, comparing changes, and organizing files and shortcuts from the terminal.

---

## 2. Reading, Searching & Comparing Files
Instead of opening a text editor with a mouse, Linux gives fast command-line tools to inspect file contents:

* **`cat` (Concatenate):** Prints the entire contents of a file straight onto the terminal screen. It works with both relative paths (`cat notes.txt`) and absolute paths (`cat /var/log/syslog`).
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

## 5. Daily Takeaway
Combining these commands makes the terminal feel practical. Being able to list hidden files (`ls -a`), filter thousands of lines of text in seconds (`grep`), and understand how symlinks connect files across the system ties everything from navigation together.
