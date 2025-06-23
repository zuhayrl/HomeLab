Great! Here's the first batch: **`grep`**, **`strings`**, and **`regex`** — structured like workflows and practical cheat sheets for real usage.

---

# 🔎 `grep` CHEAT SHEET (Search Within Text Files)

### 📌 Basic Usage

```bash
grep "pattern" file.txt
grep -i "pattern" file.txt       # Case-insensitive
grep -r "pattern" /path/         # Recursive search
grep -l "pattern" *.txt          # Show filenames only
grep -n "pattern" file.txt       # Show line numbers
grep -v "pattern" file.txt       # Invert match (show non-matching lines)
```

### 📌 Extended Search

```bash
grep -E "foo|bar" file.txt       # OR pattern using extended regex
grep -e "pattern1" -e "pattern2" file.txt  # Multiple patterns
```

### 📌 Context Around Matches

```bash
grep -A 2 "error" file.txt       # 2 lines After match
grep -B 2 "error" file.txt       # 2 lines Before match
grep -C 3 "error" file.txt       # 3 lines Before and After
```

### 📌 With Regex (More below)

```bash
grep -E "^[0-9]{3}-[0-9]{3}-[0-9]{4}$" phones.txt  # US phone format
grep -P "\d{4}-\d{2}-\d{2}" logs.txt              # Perl regex for dates
```

### 📌 Common flags

| Flag | Meaning                |
| ---- | ---------------------- |
| `-r` | Recursive              |
| `-l` | Output file names only |
| `-c` | Count matching lines   |
| `-n` | Line numbers           |
| `-w` | Match whole words only |
| `-x` | Match whole lines only |

---

# 🧵 `strings` CHEAT SHEET (Extract Printable Strings)

### 📌 Basic Usage

```bash
strings binaryfile
strings -n 6 file.bin        # Minimum string length = 6
```

### 📌 Combine with `grep` to find sensitive info

```bash
strings app | grep "password"
strings image.iso | grep -iE "key|token|auth"
```

### 📌 Dump all strings from ELF or binary

```bash
strings -a -n 4 /usr/bin/ls
```

### 📌 Use in malware or binary inspection

```bash
strings suspicious.exe | grep -Ei "url|http|ftp|ip|host"
```

---

# 📐 `regex` CHEAT SHEET (Regular Expressions Basics)

> Syntax varies slightly by tool (`grep`, `sed`, `awk`, etc). Below uses `grep -E` (extended regex).

### 📌 Anchors

```text
^pattern      # start of line
pattern$      # end of line
```

### 📌 Character Classes

```text
[abc]         # a or b or c
[a-z]         # any lowercase letter
[^a-z]        # NOT lowercase letter
```

### 📌 Quantifiers

```text
*             # 0 or more
+             # 1 or more
?             # 0 or 1
{n}           # exactly n
{n,}          # n or more
{n,m}         # between n and m
```

### 📌 Common Patterns

```text
\d            # digit         (use -P in grep)
\w            # word char     (alnum + _)
\s            # whitespace
.             # any character except newline
```

### 📌 Grouping & Alternation

```text
(foo|bar)     # match foo or bar
```

### 📌 Examples

```bash
grep -E "^[A-Z]{3}[0-9]{4}$" ids.txt         # AAA1234 pattern
grep -P "\b\w{8,}\b" file.txt                # Words with 8+ characters
grep -oP "https?://\S+" file.txt             # URLs
```

---


