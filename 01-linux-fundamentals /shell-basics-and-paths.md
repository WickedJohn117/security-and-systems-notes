# Linux Shell Basics & File Navigation

## 1. What I Learned Today
Today I started learning how to interact with Linux entirely through the terminal (command line). Instead of clicking around folders with a mouse like in Windows or macOS, I am learning how to navigate using typed commands.

- **The Terminal Prompt (`hacker@dojo:~$`):** 
  - The `$` symbol means I am logged in as a standard, regular user (not the all-powerful admin/root).
  - The `~` symbol represents my current home directory.
- **How Commands Work:** A command is just typing its name followed by spaces and options (arguments). For example, `cd` means "change directory".

---

## 2. Moving Around the Filesystem (Paths)
One of the trickiest parts I practiced today is understanding how to tell the terminal where to go. There are two main ways to point to a file or folder:

1. **Absolute Paths:** These always start from the very top root directory (`/`). No matter where I am currently standing in the terminal, typing an absolute path like `/home/hacker` will always take me to the exact same place.
2. **Relative Paths:** These depend on where I am *right now*. 
   - `.` means the **current folder** I am standing in.
   - `..` means the **parent folder** (moving one level up).
   - `~` takes me straight back to my **home folder**.

---

## 3. Why Does `./` Matter?
A cool thing I discovered today is why I can't just type the name of a local program to run it (like typing `run` vs `./run`). 

When I type a bare command, Linux checks a special list of trusted system folders called `$PATH` to find where that program lives. For security, Linux doesn't automatically look inside the folder I am currently standing in. So, to run a custom program right in front of me, I have to explicitly tell the terminal "run it from *here*" by putting `./` in front of it.

---

## 4. Daily Takeaway
Command-line navigation felt a bit foreign at first, but practicing absolute vs. relative paths and understanding how `cd` works makes moving through Linux feel much more natural. Looking forward to building on this tomorrow!
