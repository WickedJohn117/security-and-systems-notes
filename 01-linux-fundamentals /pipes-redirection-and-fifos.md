# Pipes, Redirection, Process Substitution and FIFOs

Every process in Linux communicates through three default input/output channels. Understanding how to redirect and connect these channels is essential for working effectively on the command line.

---

## Standard Channels (`stdin`, `stdout`, `stderr`)

By default, every Linux command starts with three open communication streams mapped to file descriptors:

| FD | Channel | Short Name | Purpose |
| :- | :--- | :--- | :--- |
| `0` | Standard Input | `stdin` | Channel through which the process reads input (such as keyboard typing or a redirected file) |
| `1` | Standard Output | `stdout` | Channel through which the process outputs normal data |
| `2` | Standard Error | `stderr` | Channel through which the process outputs error messages and diagnostics |

Because `stdout` (`1`) and `stderr` (`2`) are separate channels, normal output and error messages can be sent to different places.

---

## Basic File Redirection

Redirection operators change where a command reads its input from or sends its output to:

```bash
# Send normal output (stdout) to a file, overwriting existing contents
command > output.txt

# Explicit file descriptor 1 redirection (same as >)
command 1> output.txt

# Append normal output to the end of a file without overwriting it
command >> output.txt

# Send error messages (stderr) to a separate file
command 2> errors.txt

# Read standard input (stdin) from a file instead of the keyboard
command < input.txt

---

## Combining Output and Errors (`2>&1`)

When you want both normal output and error messages saved in the same file, merge `stderr` (`2`) into `stdout` (`1`):

```bash
# Redirect stdout to combined.log first, then send stderr to the same place
command > combined.log 2>&1
```

## Basic Pipes (|) and Filtering (grep)
A pipe (|) connects the standard output (stdout) of one command directly to the standard input (stdin) of the next command without creating temporary files:

```bash
# Search a saved file for matching lines
grep "ERROR" output.txt

# Filter live output directly through grep
command | grep "ERROR"

# Include error messages in the pipe by merging stderr into stdout first
command 2>&1 | grep "error"
```

## Process Substitution (`<(...)` and `>(...)`)

Process substitution runs a command in a subshell and exposes its stream as a path (such as `/dev/fd/63`) so utilities expecting file arguments—or separate output/error consumers—can interact with live streams directly.

```bash
# Input process substitution: pass two live command outputs to a command expecting file paths
diff <(sort file_a.txt) <(sort file_b.txt)

# Output process substitution: >(...) expands to a path, so pair it with a redirection operator and space
command 2> >(error_handler) | output_handler
command > >(stdout_handler) 2> >(stderr_handler)
```

## Named Pipes / FIFOs (mkfifo)
Unlike an anonymous pipe (|), a named pipe (FIFO) exists on the filesystem as a special pipe entry (p in ls -l). Data still moves through a kernel buffer rather than staying permanently on disk, and opening a FIFO blocks until both a reader and a writer are present.

```bash
# Create a named pipe on the filesystem
mkfifo /tmp/demo.fifo

# Writing blocks until a process opens the FIFO for reading (and vice versa)
echo "sample stream" > /tmp/demo.fifo
cat /tmp/demo.fifo

# Clean up the FIFO entry when finished
rm -f /tmp/demo.fifo
```
