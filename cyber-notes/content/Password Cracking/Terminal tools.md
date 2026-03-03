---
title: Terminal tools
weight: 1
---

# Password cracking tools

## Hashcat

### Using masking 

- You can use masking in your `hashcat` command if you know some of the password (ex. A few letters at the beginning or end). The flag for this type of attack is `-a3`. 



## Examples

#### Masking

```bash
hashcat -m 0 -a 3 [file] 'flag-?d?d?d?d'
```

- In this example, we know the hash starts with the word flag and then has 4 unknown characters behind it represented by `?d`.
