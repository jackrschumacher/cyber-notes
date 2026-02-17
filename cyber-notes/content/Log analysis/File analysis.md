---
title: Terminal tools
weight: 1
---

# Parsing tools

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

-v, --invert-match
            Invert the sense of matching, to select non-matching lines.

-w, --word-regexp
            Select only those lines containing matches that form whole
            words.

-x, --line-regexp
            Select only those lines that exactly match the whole line.

-c, --count
            Suppress normal output; instead print a count of matching
            lines for each input file.

-l, --files-with-matches
            Suppress normal output; instead print the name of each
            input file from which output would normally have been
            printed.

-L, --files-without-match
            Suppress normal output; instead print the name of each
            input file from which no output would have been printed.

-m NUM, --max-count=NUM
            Stop reading a file after NUM matching lines.

-o, --only-matching
            Print only the matched (non-empty) parts of a matching
            line, with each such part on a separate output line.

-n, --line-number
            Prefix each line of output with the 1-based line number
            within its input file.

-B NUM, --before-context=NUM
            Print NUM lines of leading context before matching lines.

-A NUM, --after-context=NUM
            Print NUM lines of trailing context after matching lines.

-C NUM, --context=NUM
            Print NUM lines of output context.

-r, --recursive
            Read all files under each directory, recursively. Follows
            symbolic links only if they are on the command line.

--exclude=GLOB
            Skip files whose base name matches GLOB (using wildcard
            matching).

--include=GLOB
            Search only files whose base name matches GLOB.

-E, --extended-regexp
            Interpret PATTERNS as extended regular expressions (EREs).

-F, --fixed-strings
            Interpret PATTERNS as fixed strings, not regular expressions.

-P, --perl-regexp
            Interpret PATTERNS as Perl-compatible regular expressions
            (PCREs).

```

### Regex

| Goal                  | Pattern | Flag | Example Command            |
| :-------------------- | :------ | :--- | :------------------------- |
| **Any single char**   | `.`     | None | `grep "c.t" file`          |
| **Start of line**     | `^`     | None | `grep "^Start" file`       |
| **End of line**       | `$`     | None | `grep "End$" file`         |
| **Zero or more reps** | `*`     | None | `grep "ab*c" file`         |
| **"True" Wildcard**   | `.*`    | None | `grep "user.*login" file`  |
| **One specific char** | `[ ]`   | None | `grep "H[ae]llo" file`     |
| **One or more reps**  | `+`     | `-E` | `grep -E "go+gle" file`    |
| **Optional (0 or 1)** | `?`     | `-E` | `grep -E "colou?r" file`   |
| **Logical OR**        | `\|`    | `-E` | `grep -E "cat\|dog" file`  |
| **Exact repetitions** | `{n}`   | `-E` | `grep -E "9{3}" file`      |
| **Literal dot/star**  | `\.`    | None | `grep "example\.com" file` |

## wc

- Can be used to count lines using the `wc -l` command

## uniq

- Can be used to filter out non-unique 

## awk

## head

- Show the first lines of a file
- Can show a set number of lines using the `-n` flag

<br>

{{< button href="https://man7.org/linux/man-pages/man1/head.1.html" >}}head man page{{< /button >}}

## Sort

### Options

```
-rn, print items in reverse numerical order
```



## Examples

### Search

#### Searching for text

##### Unique text

```bash
# (Replace $N with the column number where the username is located).
awk '{print $N}' filename.log | sort | uniq
```

##### Counting text

- Use `wc -l` to count the number of times something occurs

```bash
# (Replace $N with the column number where the username is located).
awk '{print $N}' filename.log | sort | uniq | wc -l
```

##### Showing frequency of strings

```bash
awk '{print $5}' filename.log | sort | uniq -c | sort -nr
```

*   **`uniq -c`**: Adds a count prefix to each unique line.
    
*   **`sort -nr`**: Sorts the final result numerically (`-n`) in reverse (`-r`), putting the most frequent users at the top.

- Can also add more columns to provide additional information
- Also helpful to use `head` to only show the top items (and the `-n` flag to only show a number of results)

#### Searching for IP Addresses

> [!NOTE]
>
> This is intended for log files that contain addresses in columns

##### Counting unique IP addresses

 ```bash
 cat [file] | cut -d " " -f 1 | sort | uniq | wc -l
 ```

- Take the file, `cut` the first field (or whatever field contains the IP addresses), sort the unique addresses, and get a line count using `wc`
- Can modify the `cut` statement in order to look
- Can also add another `cut -d ' ' -f #` statement to print additional fields
- You can also sometimes use the `awk` command instead of the `cut` command for this purpose

##### Looking for unique IP response codes

```bash
cat [file] | cut -d '"' -f [] | cut -d ' ' -f [] | sort | uniq -c | sort -rn
```

- `cat` out the file and `cut` fields, sort, get unique values, sort in descending order using `sort -rn`



