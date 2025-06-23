# Table of Contents

---

# `grep` CHEAT SHEET (Search Within Text Files)

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

# `strings` CHEAT SHEET (Extract Printable Strings)

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

# `regex` CHEAT SHEET (Regular Expressions Basics)

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

# `awk` CHEAT SHEET (Pattern-Action Text Processing)

### 📌 Basic Structure

```bash
awk 'pattern { action }' file
```

### 📌 Common Usage

```bash
awk '{ print }' file              # Print all lines (default behavior)
awk '{ print $1 }' file           # Print first column
awk '{ print $1, $3 }' file       # Print first and third columns
awk 'NR==5' file                  # Print 5th line
awk 'NF > 3' file                 # Print lines with >3 fields
```

### 📌 With Delimiters

```bash
awk -F: '{ print $1 }' /etc/passwd   # Use `:` as field delimiter
awk -F',' '{ print $2 }' file.csv    # Print second column of CSV
```

### 📌 Built-in Variables

| Variable | Description             |
| -------- | ----------------------- |
| `$0`     | Entire current line     |
| `$1`     | First field             |
| `NF`     | Number of fields        |
| `NR`     | Current line number     |
| `FS`     | Field separator (input) |
| `OFS`    | Output field separator  |

### 📌 Practical Examples

```bash
awk '{ sum += $1 } END { print sum }' file   # Sum first column
awk 'BEGIN { print "Name\tAge" } { print $1, $2 }' file
awk '$3 > 80' students.txt                   # Filter rows by 3rd column
```

---

# `sed` CHEAT SHEET (Stream Editing & Inline Text Replacement)

### 📌 Basic Usage

```bash
sed 's/foo/bar/' file             # Replace first 'foo' with 'bar'
sed 's/foo/bar/g' file            # Replace all 'foo' with 'bar'
sed 's/foo/bar/2' file            # Replace 2nd occurrence
```

### 📌 In-place Editing

```bash
sed -i 's/error/ERROR/g' file     # Modify file directly
```

### 📌 Deleting Lines

```bash
sed '/^$/d' file                  # Delete blank lines
sed '3d' file                     # Delete line 3
sed '1,5d' file                   # Delete lines 1 to 5
```

### 📌 Print Specific Lines

```bash
sed -n '3p' file                  # Print line 3 only
sed -n '5,7p' file                # Print lines 5 to 7
```

### 📌 Multiple Commands

```bash
sed -e 's/foo/bar/' -e 's/baz/qux/' file
```

---

# `cut` CHEAT SHEET (Extract Columns from Text)

### 📌 Extract by Byte or Character

```bash
cut -b 1-5 file.txt              # First 5 bytes
cut -c 1-10 file.txt             # First 10 characters
```

### 📌 Extract by Delimiter

```bash
cut -d ':' -f 1 /etc/passwd      # First field using ":" as delimiter
cut -d ',' -f 2,3 file.csv       # Fields 2 and 3 from CSV
cut -f 1-3 file.tsv              # Default tab-delimited
```

### 📌 Combine with Pipes

```bash
ps aux | cut -d ' ' -f 1         # Extract user column
ls -l | cut -c 1-10              # Extract permissions
```

---

# `tr` CHEAT SHEET (Translate or Delete Characters)

### 📌 Basic Usage

```bash
tr 'a-z' 'A-Z' < file.txt           # Convert to uppercase
tr -d '\r' < file.txt               # Remove carriage returns
tr -s ' ' < file.txt                # Squeeze multiple spaces to one
```

### 📌 Common Character Sets

| Pattern | Meaning           |
| ------- | ----------------- |
| `a-z`   | Lowercase letters |
| `A-Z`   | Uppercase letters |
| `0-9`   | Digits            |
| `\n`    | Newline           |
| `\t`    | Tab               |

### 📌 Examples

```bash
echo "HELLO" | tr 'A-Z' 'a-z'         # hello
echo "122333" | tr -s '3'             # 1223
echo "some,text,here" | tr ',' '\n'   # one value per line
```

> `tr` works only on stdin/stdout. Use redirection or pipes.

---

# `find` CHEAT SHEET (File Search Utility)

### 📌 Basic Search

```bash
find . -name "file.txt"               # Exact filename
find . -iname "file.txt"              # Case-insensitive
find /var/log -type f -name "*.log"   # All .log files
```

### 📌 Search by Type

```bash
find . -type f                        # Regular files
find . -type d                        # Directories
find . -type l                        # Symbolic links
```

### 📌 Time-Based Search

```bash
find . -mtime -1                      # Modified in last 24h
find . -atime +7                      # Accessed more than 7 days ago
find . -newer file.txt                # Modified after file.txt
```

### 📌 Size-Based Search

```bash
find . -size +100M                    # Files > 100MB
find . -size -1k                      # Files < 1KB
```

### 📌 Execute Commands

```bash
find . -name "*.tmp" -exec rm {} \;   # Delete *.tmp files
find . -type f -exec chmod 644 {} \;  # Change permissions
```

---

# `xargs` CHEAT SHEET (Build Commands from Input)

### 📌 Basic Usage

```bash
echo "file1 file2" | xargs rm         # Equivalent to: rm file1 file2
```

### 📌 With `find`

```bash
find . -name "*.log" | xargs rm       # Delete all *.log files
```

### 📌 Handling Spaces in Filenames

```bash
find . -name "*.mp4" -print0 | xargs -0 rm
```

### 📌 Limit Arguments per Command

```bash
seq 1 100 | xargs -n 10               # Run in batches of 10 args
```

### 📌 Replace Placeholder

```bash
echo "apple banana" | xargs -I {} echo "fruit: {}"
```

---

Perfect — here's the final batch: **`head`**, **`tail`**, **`diff`**, and **`wc`**. These are essential tools for inspecting, comparing, and summarizing text files.

---

# 🔼 `head` CHEAT SHEET (First N Lines of Files)

### 📌 Basic Usage

```bash
head file.txt                     # First 10 lines (default)
head -n 5 file.txt                # First 5 lines
head -c 100 file.txt              # First 100 bytes
```

### 📌 Multiple Files

```bash
head -n 3 file1.txt file2.txt     # Show first 3 lines of both
```

---

# `tail` CHEAT SHEET (Last N Lines of Files)

### 📌 Basic Usage

```bash
tail file.txt                     # Last 10 lines (default)
tail -n 20 file.txt               # Last 20 lines
tail -c 200 file.txt              # Last 200 bytes
```

### 📌 Real-Time Log Monitoring

```bash
tail -f /var/log/syslog           # Follow live output
tail -F log.txt                   # Like -f, but handles log rotation
```

### 📌 Combine with `grep`

```bash
tail -f app.log | grep "ERROR"
```

---

# `diff` CHEAT SHEET (Compare Files Line by Line)

### 📌 Basic Usage

```bash
diff file1.txt file2.txt
```

### 📌 Interpret Output

* `<` = line from file1
* `>` = line from file2
* `c` = change, `a` = add, `d` = delete

### 📌 Unified Format (more readable)

```bash
diff -u file1.txt file2.txt
```

### 📌 Side-by-Side Comparison

```bash
diff -y file1.txt file2.txt
```

### 📌 Ignore Case/Whitespace

```bash
diff -i file1 file2      # Ignore case
diff -b file1 file2      # Ignore whitespace
```

---

# `wc` CHEAT SHEET (Word, Line, Character Count)

### 📌 Basic Usage

```bash
wc file.txt                     # Lines, words, bytes
```

### 📌 Specific Counts

```bash
wc -l file.txt                  # Line count
wc -w file.txt                  # Word count
wc -c file.txt                  # Byte count
wc -m file.txt                  # Character count
```

### 📌 Multiple Files

```bash
wc -l file1 file2              # Show line counts per file
```

### 📌 Combine with Other Tools

```bash
grep "ERROR" log.txt | wc -l   # Count error lines
find . -type f | wc -l         # Count number of files
```

---

# `cat` CHEAT SHEET (Concatenate and Display Files)

### 📌 Basic Usage

```bash
cat file.txt                   # Print contents to stdout
cat file1.txt file2.txt        # Concatenate and print both files
```

### 📌 Redirect Output

```bash
cat file1.txt > combined.txt          # Overwrite combined.txt
cat file1.txt >> combined.txt         # Append to combined.txt
```

### 📌 Create a New File via Terminal

```bash
cat > newfile.txt
# Type lines, then Ctrl+D to save
```

### 📌 Concatenate Files

```bash
cat header.txt body.txt footer.txt > full.txt
```

---

### 📌 Numbering Lines

```bash
cat -n file.txt                       # Number all lines
cat -b file.txt                       # Number non-blank lines
```

### 📌 Show Hidden Characters

```bash
cat -T file.txt                       # Show TABs as ^I
cat -E file.txt                       # Show line ends as $
```

### 📌 Combine Flags

```bash
cat -n -T file.txt                    # Number lines, show TABs
```

---

### 📌 Practical Workflows

| Task                           | Command                                  |               |
| ------------------------------ | ---------------------------------------- | ------------- |
| View contents of a config      | `cat ~/.bashrc`                          |               |
| Combine logs                   | `cat part1.log part2.log > full.log`     |               |
| Add newlines between two files | `cat file1 <(echo '') file2` (bash only) |               |
| Check file ends with newline   | \`tail -1 file.txt                       | od -c\`       |
| Preview a binary (partial)     | \`cat binaryfile                         | head -c 100\` |

---

### ⚠️ Caution: Use with `sudo`

```bash
# This won't work as intended:
sudo cat file.txt > /root/output.txt

# Instead, use:
sudo sh -c 'cat file.txt > /root/output.txt'
```

---


# `tac` CHEAT SHEET (Reverse `cat` — Print Lines in Reverse)

### 📌 Basic Usage

```bash
tac file.txt                      # Print file from bottom to top
```

### 📌 Combine with Other Tools

```bash
tac file.txt | grep "ERROR"       # Search latest logs first
tac file.txt | head -n 10         # Last 10 lines in reverse
```

### 📌 Reverse multiple files

```bash
tac file1.txt file2.txt
```

---

# 📖 `more` CHEAT SHEET (Paginated File Viewer – Older, Simpler)

### 📌 Basic Usage

```bash
more file.txt
```

### 📌 Keyboard Shortcuts (inside `more`)

| Key        | Action          |
| ---------- | --------------- |
| SPACE      | Next page       |
| ENTER      | Next line       |
| `q`        | Quit            |
| `/pattern` | Search forward  |
| `n` / `N`  | Next/prev match |

### 📌 Other Tips

```bash
more +20 file.txt         # Start from line 20
command | more            # Page output of any command
```

---

# 📚 `less` CHEAT SHEET (Enhanced Pager – More Powerful than `more`)

### 📌 Basic Usage

```bash
less file.txt
```

### 📌 Navigation

| Key     | Action                  |
| ------- | ----------------------- |
| `j`/`k` | Scroll down/up one line |
| `SPACE` | Scroll down one page    |
| `b`     | Scroll up one page      |
| `G`     | Go to end of file       |
| `g`     | Go to top of file       |
| `q`     | Quit                    |

### 📌 Searching

```bash
/pattern         # Forward search
?pattern         # Backward search
n / N            # Next/previous match
```

### 📌 View compressed files directly

```bash
less file.gz
```

### 📌 Live view of logs (similar to tail -f)

```bash
less +F log.txt     # Follow mode, press Ctrl+C to stop following
```

### 📌 Combine with Commands

```bash
ps aux | less
git log | less
```

---

### 🔄 `cat` vs `tac` vs `more` vs `less`

| Command | Direction    | Paging | Search  | Notes                        |
| ------- | ------------ | ------ | ------- | ---------------------------- |
| `cat`   | Top → bottom | ❌      | ❌       | Use for full file dumps      |
| `tac`   | Bottom → top | ❌      | ❌       | Reverse line order           |
| `more`  | Top → bottom | ✅      | Limited | Legacy pager, line-by-line   |
| `less`  | Both         | ✅      | ✅       | Modern pager, scroll, search |





