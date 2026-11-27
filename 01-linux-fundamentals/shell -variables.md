# Shell Variables

## 1. Setting and Reading Variables
* Assigning values in Bash (`VAR=value` with **no spaces** around `=`) and reading them with `$VAR` or `"$VAR"`.
* Why quoting matters when values contain spaces or special characters so words do not split unexpectedly.

## 2. Shell Scope vs. Environment Variables (`export`)
* Local shell variables exist in the current shell session; using `export VAR` passes the variable into child processes spawned from that shell.
* Why child processes cannot push variable changes back up into their parent shell.

## 3. Capturing Output and Reading Input
* **Command Substitution (`$(...)`):** Capturing command output into a variable instead of copying it manually.
* **Reading Input & Redirection (`read`):** Reading values interactively or feeding a file/stream directly into `read VAR < file` without extra subshell overhead.

## 4. Security & Operational Takeaways
* **Checking Key Variables (`$PATH` and Environment State):** Inspecting variables like `PATH` or environment settings before running scripts avoids unexpected command resolution or accidental overwrites.
* **Avoiding Accidental Leaks or Clobbering:** Being careful when printing, exporting, or reusing variable names keeps your shell session predictable and avoids overwriting important configuration values.
