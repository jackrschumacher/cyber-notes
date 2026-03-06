---
title: Terminal tools
weight: 1
---

# Password cracking tools

## Hashcat

### Using masking 

- You can use masking in your `hashcat` command if you know some of the password (ex. A few letters at the beginning or end). The flag for this type of attack is `-a3`. 

### Cracking pdf hashes using Hashcat

```bash
hashcat [file] -O -m 10700 
hashcat [file] -m 10700 --show #shows the cracked password
```

`-O` speeds up the cracking process by using the optimized kernel

## John the Ripper

### pdf2john

```bash
pdf2john [PDF file] > [Output file.txt]
```

- Outputs the hash of the selected pdf into the output file

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
