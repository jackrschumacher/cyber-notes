---
title: Terminal tools
weight: 1
---

# Password cracking tools

## Hashcat

### Options
### Attack Modes 
Hashcat uses numbers to define how it should attack the hash.

| Mode | Name | Description |
| :--- | :--- | :--- |
| `-a 0` | **Straight** | Standard dictionary attack (tests words from a list). |
| `-a 1` | **Combination** | Combines words from multiple dictionaries together. |
| `-a 3` | **Brute-force / Mask** | Guesses characters based on a specific pattern/mask. |
| `-a 6` | **Hybrid Dict + Mask** | Appends characters to dictionary words (e.g., `password123`). |
| `-a 7` | **Hybrid Mask + Dict** | Prepends characters to dictionary words (e.g., `123password`). |

---

### Core Cracking Commands

| Command | Description | Example |
| :--- | :--- | :--- |
| **Dictionary Attack** | Basic attack using a wordlist. | `hashcat -m 11600 -a 0 hash.txt rockyou.txt` |
| **Wordlist + Rules** | Applies mutation rules (like John's `--rules`). Hashcat uses rule files. | `hashcat -m 11600 -a 0 hash.txt rockyou.txt -r /usr/share/hashcat/rules/best64.rule` |
| **Mask Attack** | Tests a specific pattern. (e.g., `?u?l?l?l?d?d?s` = 1 Upper, 3 Lower, 2 Digits, 1 Symbol). | `hashcat -m 11600 -a 3 hash.txt ?u?l?l?l?l?d?d?s` |
| **Show Cracked** | Displays passwords that have already been cracked in the current session. | `hashcat -m 11600 --show hash.txt` |
| **Restore Session** | Resumes a previously interrupted cracking session. | `hashcat --restore` |
| **Benchmark** | Tests how fast your hardware can crack a specific hash type. | `hashcat -b -m 11600` |
| **Force CPU** | Forces Hashcat to use the CPU (useful on Raspberry Pi if OpenCL/GPU drivers fail). | `hashcat -m 11600 -a 0 hash.txt rockyou.txt --force -D 1` |

---

### Common Hash Modes 
You must tell Hashcat exactly what mathematical algorithm to use. You can find the full list by running `hashcat --help`.

| ID (`-m`) | Algorithm / File Type |
| :--- | :--- |
| `11600` | 7-Zip (`.7z`) |
| `13600` | WinZip (Legacy PKZIP) |
| `13000` | RAR5 (`.rar`) |
| `1000` | NTLM (Windows Passwords) |
| `0` | MD5 |
| `1400` | SHA256 |
| `3200` | bcrypt (Common Web Hashes) |


## John the Ripper

### Options

| Command | Description | Example |
| :--- | :--- | :--- |
| **Dictionary Attack** | Tests passwords against a specific wordlist (like `rockyou.txt`). | `john --wordlist=rockyou.txt hash.txt` |
| **Wordlist + Rules** | Applies mutation rules (adding numbers, capitalization) to a wordlist. | `john --wordlist=rockyou.txt --rules hash.txt` |
| **Show Cracked** | Displays passwords that have already been successfully cracked. | `john --show hash.txt` |
| **Specify Format** | Forces John to use a specific hash type (e.g., NT, Raw-MD5, bcrypt) instead of auto-detecting. | `john --format=NT hash.txt` |
| **Single Crack Mode** | Uses information derived from the username/GECOS fields to guess passwords. Fast and efficient for initial runs. | `john --single hash.txt` |
| **Incremental Mode** | Pure brute-force attack testing all character combinations. Can take a massive amount of time. | `john --incremental hash.txt` |
| **Restore Session** | Resumes a previously interrupted cracking session. | `john --restore` |
| **List Formats** | Displays all the hash algorithms supported by your current build. | `john --list=formats` |
| **Test Performance** | Runs a benchmark to test how many passwords per second your CPU can guess for a specific format. | `john --test --format=7z` |



### john tools

| Tool     | Purpose/File decoded                      | Command                          |
| -------- | ----------------------------------------- | -------------------------------- |
| pdf2john | Extract password hash from `.pdf` files   | `pdf2john [PDF file] > [output]` |
| zip2john | Extract password hash from `.zip` archive | `zip2john [ZIP file] > [output]` |
| 7z2john  | Extract password hash from `.7z` archive  | `7z2john [7z file] > [output]`   |

### Using the wordlist (with multi-threading)

```bash
john --wordlist=[wordlist location] [file to crack] --fork=$(nproc) #Use all avaliable processors on the machine
```

- Uses all available cpu cores to compare the password against the wordlist 





## Examples

#### Masking

```bash
hashcat -m 0 -a 3 [file] 'flag-?d?d?d?d'
```

- In this example, we know the hash starts with the word flag and then has 4 unknown characters behind it represented by `?d`.
