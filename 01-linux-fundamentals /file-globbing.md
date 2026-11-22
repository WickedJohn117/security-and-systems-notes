# Linux Fundamentals: File Globbing & Shell Expansion

**Module:** Linux Luminarium — File Globbing (`pwn.college`)  
**Focus:** Pattern matching, wildcard expansion, exclusionary character classes, and CLI tab completion.

---

## 1. Core Globbing Mechanisms

In Bash, **globbing** (pathname expansion) happens *before* a command executes. The shell expands wildcard patterns into matching filenames and passes that list as arguments to the target program. If a pattern matches zero files, Bash passes the raw pattern string literally (unless `nullglob` is enabled).

| Operator / Syntax | Behavior | Example | Matches |
| :--- | :--- | :--- | :--- |
| `*` | Matches **zero or more** characters (excluding leading `.` dotfiles by default) | `*.log` | `auth.log`, `syslog`, `.log` (if hidden) |
| `?` | Matches **exactly one** single character | `file?.txt` | `file1.txt`, `fileA.txt` (not `file10.txt`) |
| `[...]` | Matches **one single character** from the enclosed set or range | `log_[0-9].txt` | `log_0.txt` through `log_9.txt` |
| `[!...]` / `[^...]` | **Exclusionary glob:** matches one single character *not* in the set | `[!pwn]*` | Any non-hidden file not starting with `p`, `w`, or `n` |

---

## 2. Key Technical Rules & Edge Cases

* **Single-Position Evaluation in `[...]`:** Characters inside square brackets represent candidates for **one character slot**, not whole words or multi-character wildcards. Putting `*` inside `[...]` treats `*` as a literal asterisk rather than a wildcard.
* **Path Expansion:** Globs work across directory boundaries (e.g., `/var/log/*/*.log` or `/challenge/[a-c]*/flag`).
* **Mixing & Chaining Globs:** Operators combine sequentially across a filename structure (e.g., `[!a-c]*_[0-9]?.txt` filters the first character, allows variable middle text, and enforces a two-character numeric suffix).
* **Safe Previewing:** Always test destructive or strict globs with `echo <pattern>` before passing them to commands like `rm`, `mv`, or restricted binaries to verify exact shell expansion.

---

## 3. Tab Completion (`Tab`)

Tab completion queries the shell's readline engine to resolve partial paths, filenames, and executable names in `$PATH`:

* **Single `Tab`:** Auto-completes immediately if there is a unique match; otherwise completes up to the longest common prefix.
* **Double `Tab` (`Tab` + `Tab`):** Lists all candidate files, directories, or commands matching the current prefix when multiple options exist.
* **Command vs. Argument Completion:** In the first token position, `Tab` searches executable binaries and shell builtins; in subsequent positions, it searches filesystem paths (or context-specific arguments).

---

## 4. Security & Operational Takeaways

* **SOC / Incident Response:** Exclusionary globs (`[!...]`) and character ranges allow rapid triage of noisy directories (e.g., isolating non-standard log prefixes or timestamped artifacts without writing complex regex pipelines).
* **Offensive / Pentesting:** Understanding pre-execution shell expansion is critical when bypassing basic input filters, enumerating restricted directories without `ls` (`echo *`), or avoiding wildcard injection pitfalls (`--` flag confusion when filenames begin with `-`).
