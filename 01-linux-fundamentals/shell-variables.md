# Shell Variables

## 1. Setting and Reading Variables

* Assigning values in Bash (`VAR=value` with **no spaces** around `=`) and reading them with `$VAR` or `"$VAR"`.
* Why quoting matters when values contain spaces or special characters so words do not split unexpectedly.

```bash
GREETING="hello from bash"
echo $GREETING       # words split/normalize whitespace when unquoted
echo "$GREETING"     # preserves exact spacing and value
```
---

## 2. Shell Scope vs. Environment Variables (`export`)
* Local shell variables exist in the current shell session; using `export VAR` passes the variable into child processes spawned from that shell.
* Why child processes cannot push variable changes back up into their parent shell.
  
  ```bash
  LOCAL_VAR="local-only"
  export ENV_VAR="shared-with-children"

  bash -c 'echo "Child sees LOCAL_VAR=${LOCAL_VAR:-<unset>} and ENV_VAR=$ENV_VAR"'
  ```
---

## 3. Capturing Output and Reading Input
* **Command Substitution (`$(...)`):** Capturing command output into a variable instead of copying it manually.
* **Reading Input & Redirection (`read`):** Reading values interactively or feeding a file/stream directly into `read VAR < file` without extra subshell overhead.

  ```bash
  CURRENT_DIR="$(pwd)"
  printf "sample line\n" > /tmp/var_input.txt
  read -r LINE_FROM_FILE < /tmp/var_input.txt
  echo "Dir: $CURRENT_DIR | Line: $LINE_FROM_FILE"
  rm -f /tmp/var_input.txt
  ```
---

## 4. Security & Operational Takeaways
* **Checking Key Variables (`$PATH` and Environment State):** Inspecting variables like `PATH` or environment settings before running scripts avoids unexpected command resolution or accidental overwrites.
* **Avoiding Accidental Leaks or Clobbering:** Being careful when printing, exporting, or reusing variable names keeps your shell session predictable and avoids overwriting important configuration values.
  ```bash
  echo "$PATH"
  env | grep -E '^(PATH|HOME|USER)='
  ```
