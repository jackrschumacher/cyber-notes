---
title: rockyou.txt walkthrough
author: Jack Schumacher
date: 2026-02-21
---

# Rockyou.txt walkthrough

| **Challenge Type** | **Difficulty** |     **Flag format/questio**      |
| :----------------: | :------------: | :------------------------------: |
| Password Cracking  |      Easy      | The text of the cracked password |



## Challenge summary

In this challenge, you will use the `rockyou.txt` password wordlist to crack a list of hashed passwords stated to be "hacker passwords". 

<img src="/images/kali-linux-2025.4-vmware-amd64_-_VMware_Workstatio_02_22_26_12-00-11 AM.png" alt="kali-linux-2025.4-vmware-amd64_-_VMware_Workstatio_02_22_26_12-00-11 AM" style="zoom:67%;" />

## Setup

To use the `rockyou` wordlist, or some other wordlists in kali you may need to extract the file because it is so large and it is reduced in size to keep the OS image light. 

## Instructions

1. First you must extract the passwords and save them to a file so it is more easy to pass the files into `hashcat` to identify the hash type in the next step. To do this you could use a simple notepad application to create the `.txt` file or could use command line tools like `nano` or `micro`.

   <img src="/images/kali-linux-2025.4-vmware-amd64_-_VMware_Workstatio_02_22_26_12-07-07 AM.png" alt="kali-linux-2025.4-vmware-amd64_-_VMware_Workstatio_02_22_26_12-07-07 AM" style="zoom:75%;" />

2. To identify the hash, you can use the `hashcat` terminal command built into Kali. To do this, simply use the terminal command, replacing `rockyouhash.txt` with the name of your own file:

   ```bash
   hashcat --identify rockyouhash.txt
   ```

   [It can also be helpful to view the `hashcat `documentation to view example hashes](https://hashcat.net/wiki/doku.php?id=example_hashes)

3. The results from the `hashcat` command should appear similar to this:

   ```
   The following 12 hash-modes match the structure of your input hash:
   
         # | Name                                                       | Category
     ======+============================================================+======================================
       900 | MD4                                                        | Raw Hash
         0 | MD5                                                        | Raw Hash
        70 | md5(utf16le($pass))                                        | Raw Hash
      2600 | md5(md5($pass))                                            | Raw Hash salted and/or iterated
      3500 | md5(md5(md5($pass)))                                       | Raw Hash salted and/or iterated
      4400 | md5(sha1($pass))                                           | Raw Hash salted and/or iterated
     20900 | md5(sha1($pass).md5($pass).sha1($pass))                    | Raw Hash salted and/or iterated
     32800 | md5(sha1(md5($pass)))                                      | Raw Hash salted and/or iterated
      4300 | md5(strtoupper(md5($pass)))                                | Raw Hash salted and/or iterated
      1000 | NTLM                                                       | Operating System
      9900 | Radmin2                                                    | Operating System
      8600 | Lotus Notes/Domino 5                                       | Enterprise Application Software (EAS)
   
   ```

   From this output, we can identify that this is likely and MD5 hash. This is one of the most common hash types and can be easily cracked using `hashcat`. 

   

4. We will now use `hashcat` to decrypt the hash. 

   To learn more about `hashcat `and its various options you can use the `hashcat --help` command to see the various options that can be used. Doing research, we can see that we are going to use the `md5` hash mode and perform a dictionary attack using the `rockyou.txt wordlist`. The command format for this looks like:

   ```bash
   hashcat -m [type] -a [attack type] [hashfile.txt] [wordlist (if applicable)] 
   ```

   So in this case, we should run the command associated with an `md5` hash and a dictionary attack. In this case this involves the flags `-m 0` specifying an `md5` hash, and the `-a 0` flag specifying a dictionary attack, with us using the `rockyou.txt` wordlist built into kali at this path: `/usr/share/wordlists/rockyou.txt`. 

   ```bash
   hashcat -m 0 -a 0 rockyouhash.txt /usr/share/wordlists/rockyou.txt
   ```

5. After completing the run, `hashcat` should return something like the following:

   ```
   [REMOVE]   
                                                             
   Session..........: hashcat
   Status...........: Cracked
   Hash.Mode........: 0 (MD5)
   Hash.Target......: rockyouhash.txt
   Time.Started.....: Sun Feb 22 02:34:24 2026 (2 secs)
   Time.Estimated...: Sun Feb 22 02:34:26 2026 (0 secs)
   Kernel.Feature...: Pure Kernel (password length 0-256 bytes)
   Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
   Guess.Queue......: 1/1 (100.00%)
   Speed.#01........:  4321.9 kH/s (0.21ms) @ Accel:1024 Loops:1 Thr:1 Vec:8
   Recovered........: 5/5 (100.00%) Digests (total), 5/5 (100.00%) Digests (new)
   Progress.........: 8732672/14344385 (60.88%)
   Rejected.........: 0/8732672 (0.00%)
   Restore.Point....: 8728576/14344385 (60.85%)
   Restore.Sub.#01..: Salt:0 Amplifier:0-1 Iteration:0-1
   Candidate.Engine.: Device Generator
   Candidates.#01...: ddrf1 -> dcsrule1
   Hardware.Mon.#01.: Util: 37%
   
   Started: Sun Feb 22 02:33:57 2026
   Stopped: Sun Feb 22 02:34:27 2026
   
   ```
   
   Analyzing these results, we can see the results of the cracked passwords.
   
   ```
   [REMOVED]
   ```

   This is our answer to the challenge- make sure to match the hash with the instructions.
