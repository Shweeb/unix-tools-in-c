# The Hands — C Track

**Purpose:** kill the sentence "I couldn't make a damn thing in C."
**Rule:** completion = working code, not pages read. No book front-to-back. K&R is a *lookup table*, not a novel.
**Estimated:** ~25–35 hours across all 10 units. Week 1 realistically covers units 1–5.

---

## Setup (do once, 20 mins)

- `gcc` or `clang` installed
- Compile with warnings on **always**: `gcc -Wall -Wextra -g -o prog pr
og.c`
- `valgrind` installed — you will use it, and it's the thing that teaches you memory
- One folder, one `Makefile`, one repo. Push it. This repo *is* a CV piece.

---

## Phase 1 — Ground (units 1–3)

### Unit 1 — Argument echo
Write a program that prints each of its command-line arguments on its own line, numbered.
**Done when:** `./echoargs a b c` prints three numbered lines and `./echoargs` alone doesn't crash.
**You now understand:** `main`, `argc`, `argv`, the compile/run loop.

### Unit 2 — `cat`
Read files named on the command line and print them to stdout. Fall back to stdin if no args.
Do it twice: once with `fopen`/`fgets`, then rewrite using `open`/`read`/`write` syscalls directly.
**Done when:** `./mycat file.txt` matches real `cat`, and `echo hi | ./mycat` works.
**You now understand:** buffered vs raw I/O, file descriptors, why `read` returns a count.

### Unit 3 — `wc`
Count lines, words and bytes.
**Done when:** output matches system `wc` on three different files, including one with no trailing newline.
**You now understand:** state machines over a byte stream, edge cases.

---

## Phase 2 — Memory (units 4–6)
*This is the phase that actually changes how you think. Do not skip it.*

### Unit 4 — Dynamic array
A `struct` with `int *data; size_t len; size_t cap;` plus `push`, `pop`, `get`, `free`.
Grow by doubling when full.
**Done when:** pushing 100,000 ints works and `valgrind --leak-check=full` reports zero leaks.
**You now understand:** `malloc`, `realloc`, `free`, ownership, why `std::vector` and `Vec<T>` exist.

### Unit 5 — String toolkit
By hand, no `string.h`: `my_strlen`, `my_strcpy`, `my_strdup`, and a `split(str, delim)` returning a NULL-terminated array of strings.
**Done when:** splitting `"a,b,,c"` gives the right four tokens, and freeing the result is valgrind-clean.
**You now understand:** null termination, pointer arithmetic, who owns returned memory. This is the single most common C interview topic.

### Unit 6 — Hash map
String keys, int values. Chaining is easier than open addressing — start there.
**Done when:** it powers a word-frequency counter over a real text file and prints the top 10 words.
**You now understand:** hashing, collisions, load factor, and you've built the data structure most languages hide from you.

---

## Phase 3 — The system (units 7–10)

### Unit 7 — `grep`
Literal substring match first. Then add `-i` (case insensitive) and `-n` (line numbers).
**Done when:** `./mygrep -n error log.txt` prints matching lines with numbers.
**Optional stretch:** support `.` and `*` wildcards. This is a genuinely famous small problem and a great interview story.

### Unit 8 — `ls`
`opendir`, `readdir`, `stat`. Print names, and with `-l` print sizes and permissions.
**Done when:** `./myls -l` output is recognisably close to real `ls -l`.
**You now understand:** the filesystem API, `struct stat`, mode bits.

### Unit 9 — `head` and `tail`
`head -n 20` is easy. `tail -n 20` is the interesting one — you can't know where the last 20 lines start until you've read the file.
**Done when:** both work with an `-n` flag, and `tail` doesn't load a 1GB file into memory.
**You now understand:** seeking, buffering strategy, thinking about memory before writing code.

### Unit 10 — Capstone: a tiny shell
Read a line, split it, `fork`, `execvp`, `wait`. Then add a single pipe: `ls | wc -l`.
**Done when:** it runs commands interactively, handles `exit`, and one pipe works.
**You now understand:** processes, file descriptor duplication (`dup2`), the actual mechanics of how your terminal works. **This is a portfolio piece.** It's also the thing you talk about for 20 minutes in an interview.

---

## Tools to pick up along the way (don't front-load these)

- **valgrind** — from unit 4 onward, run it every time
- **gdb** — learn exactly four commands when you first hit a segfault: `run`, `bt`, `break`, `print`
- **Makefile** — one target, from unit 3. Ten lines, no more
- **man pages** — `man 2 read`, `man 3 malloc`. Section 2 is syscalls, section 3 is library functions

---

## Rules of engagement

1. **Timebox, don't perfect.** If a unit takes more than ~4 hours, ship it working-but-ugly and move on. You can come back.
2. **Stuck for 30 minutes → look it up.** Struggling builds skill; grinding builds resentment.
3. **Commit after every unit.** The git history is evidence of consistency, which is what a hiring manager is actually reading for.
4. **Don't read ahead.** The impulse to "properly learn C first" is the same impulse that stalled you on the algorithms book.
5. **Write a one-paragraph README per unit** — what it does, what you learned, what you'd do differently. Ten paragraphs at the end is a blog post, and a blog post is a CV asset.

---

## After this track

- **C++ track** — the same tools rewritten with RAII, `std::vector`, `std::string`, smart pointers. You'll *feel* what C++ is protecting you from, which is the only way it makes sense.
- **Rust track** — the Embedded Rust Book plus the Pico. The borrow checker stops being mysterious once you've manually freed things wrong a few times.
- **Kernel track** — pick the OS project back up. Interrupts and a basic scheduler are the natural next units.
- **DSA/interview prep** — do this *after* the hands work. Solving algorithm problems in a language you're afraid of is misery.
