---
title: Log analysis tools
---

# Tools

## grep

- You can use grep in order to search through files looking for key words like ssh when analyzing logs

```bash
grep '[search]' [filename.txt] 
```

<br>

{{< button href="https://man7.org/linux/man-pages/man1/grep.1.html" >}}Grep Man Page{{< /button >}}

### Options

```
-i, --ignore-case
              Ignore case distinctions in patterns and input data, so
              that characters that differ only in case match each other.
-n, --line-number
              Prefix each line of output with the 1-based line number
              within its input file.
-v, --invert-match
          Invert the sense of matching, to select non-matching lines.

```

## wc

## uniq

- Can be used to filter out non-unique 

## awk

## head

- Show the first lines of a file
- Can show a set number of lines using the `-n` flag

<br>

{{< button href="https://man7.org/linux/man-pages/man1/head.1.html" >}}head man page{{< /button >}}

# Examples

## Search

### Searching for usernames

#### Unique usernames

```bash
# (Replace $N with the column number where the username is located).
awk '{print $N}' filename.log | sort | uniq
```

##### Counting usernames

- Use `wc -l` to count the number of times something occurs

```bash
# (Replace $N with the column number where the username is located).
awk '{print $N}' filename.log | sort | uniq | wc -l
```

##### Showing frequency of usernames

```bash
awk '{print $5}' filename.log | sort | uniq -c | sort -nr
```

*   **`uniq -c`**: Adds a count prefix to each unique line.
    
*   **`sort -nr`**: Sorts the final result numerically (`-n`) in reverse (`-r`), putting the most frequent users at the top.

- Can also add more columns to provide additional information
