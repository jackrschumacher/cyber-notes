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

## Ophcrack

### Ophcrack CLI Options

While Ophcrack is most famous for its graphical interface, it also has a command-line interface (CLI) that functions similarly to the table you provided. Here is a breakdown of common Ophcrack CLI commands and flags:

| **Command**                    | **Description**                                              | **Example**                             |
| ------------------------------ | ------------------------------------------------------------ | --------------------------------------- |
| **Load PWDump File**           | Loads target Windows hashes from a standard PWDump formatted text file. | `ophcrack -f hashes.txt`                |
| **Specify Table Directory**    | Sets the root directory where your pre-computed Rainbow Tables are stored. | `ophcrack -d /path/to/tables/`          |
| **Load from SAM**              | Extracts and loads hashes directly from a local Windows SAM and SYSTEM registry hive directory. | `ophcrack -w /Windows/System32/config/` |
| **Select Specific Tables**     | Specifies exactly which installed rainbow tables to activate for the current cracking session. | `ophcrack -t XP_free_fast,Vista_free`   |
| **Set Thread Count**           | Defines the number of CPU threads Ophcrack will use, improving performance on multi-core systems. | `ophcrack -n 4 -f hashes.txt`           |
| **Export Results**             | Exports the successfully cracked passwords and session details to a specified CSV file. | `ophcrack -f hashes.txt -x results.csv` |
| **Disable GUI (Console Mode)** | Forces Ophcrack to run entirely in the terminal interface without launching the graphical window. | `ophcrack -s -f hashes.txt`             |
| **Display Help**               | Prints the full list of available command-line arguments and usage instructions. | `ophcrack -h`                           |

## Aircrack-ng Suite

### Core Cracking Commands (aircrack-ng)

Aircrack-ng is the primary cracking tool within the suite, used to recover WEP and WPA/WPA2-PSK keys once enough data packets or a 4-way handshake has been captured.

| Command | Description | Example |
| :--- | :--- | :--- |
| **Dictionary Attack (WPA/WPA2)** | Cracks WPA/WPA2 passwords using a captured handshake and a wordlist. | `aircrack-ng -w rockyou.txt capture.cap` |
| **Target Specific BSSID** | Targets a specific MAC address (BSSID) if the capture file contains multiple networks. | `aircrack-ng -b 00:11:22:33:44:55 capture.cap` |
| **Target Specific ESSID** | Targets a specific network name (ESSID) in the capture file. | `aircrack-ng -e "MyNetwork" capture.cap` |
| **Standard WEP Cracking** | Cracks WEP keys using the PTW approach (requires sufficient IVs captured). | `aircrack-ng -a 1 capture.cap` |
| **Save/Restore Session** | Saves the current cracking progress to a file, or restores it later. | `aircrack-ng -w rockyou.txt -N session.db capture.cap` |

---

### Aircrack-ng Suite Tools

Unlike Hashcat or John the Ripper, Aircrack-ng is a full suite of tools designed to handle every step of the wireless auditing process, from putting the interface into monitor mode to capturing the required handshakes.

| Tool | Purpose | Command / Example |
| :--- | :--- | :--- |
| **airmon-ng** | Manages wireless interfaces. Used to enable or disable monitor mode, allowing the card to capture all wireless traffic. | `airmon-ng start wlan0` |
| **airodump-ng** | The packet sniffer. Used to scan for target networks and capture their packets (specifically the WPA 4-way handshake) to a `.cap` file. | `airodump-ng -c 6 --bssid 00:11:22:33:44:55 -w capture wlan0mon` |
| **aireplay-ng** | Injects frames. Most commonly used to send deauthentication (deauth) packets to force a client to disconnect and reconnect, generating a handshake. | `aireplay-ng -0 5 -a [Router MAC] -c [Client MAC] wlan0mon` |
| **airbase-ng** | Used for attacking clients rather than the Access Point. Can create rogue Access Points (Evil Twin attacks). | `airbase-ng -e "Free WiFi" -c 6 wlan0mon` |

---

## Examples

#### Full WPA/WPA2 Cracking Workflow

To crack a WPA/WPA2 network, you typically use three tools from the suite in sequence:

1. **Start Monitor Mode:**
   ```bash
   airmon-ng start wlan0
   ```
2. **Capture the Handshake (Leave this running):**
   ```bash
   airodump-ng -c [Channel] --bssid [Target MAC] -w mycapture wlan0mon
   ```
3. **Force a Handshake (In a new terminal window):**
   ```bash
   aireplay-ng -0 2 -a [Target MAC] -c [Client MAC] wlan0mon
   ```
   *Note: `-0 2` sends 2 deauthentication packets to temporarily kick the client off the network.*
4. **Crack the Captured Hash:**
   ```bash
   aircrack-ng -w /usr/share/wordlists/rockyou.txt mycapture-01.cap
   ```
   *Once `airodump-ng` displays "WPA handshake: [MAC]", you can stop the capture and run `aircrack-ng` to compare the handshake against your wordlist.*


## Examples

#### Masking

```bash
hashcat -m 0 -a 3 [file] 'flag-?d?d?d?d'
```

- In this example, we know the hash starts with the word flag and then has 4 unknown characters behind it represented by `?d`.
