---
title: TryHackMe:Crack The Hash
description: Writeup for TryHackMe challenge-Crack The Hash
author: Prabin Shrestha
date: 2025-03-25 11:33:00 +0800
categories: [Writeup, TryHackMe]
tags: [Writeup]
pin: false
math: true
mermaid: true

---

# Task 1 Walkthrough

This is a walk-through for cracking different hashes using Hashcat on macOS.

## Tools Required:
- Hashcat
- Online Hash Type Identifier: [hashes.com](https://hashes.com/en/tools/hash_identifier) -To identify the Hash Type
- Hash Mode Lookup: [Hashcat Example Hashes](https://hashcat.net/wiki/doku.php?id=example_hashes) - To Identify the hash mode to be used for the given hash
- rockyou.txt from github

## Installation of Hashcat:
1. Open terminal on macOS.
2. Run the command: `brew install hashcat`
3. Check if the hashcat is installed with the command: `hashcat --version`
4. Check the directory of the installed hashcat with the command: `which hashcat`

```bash
goldeneye10@GoldenEyes-MacBook-Pro ~ % which hashcat
/opt/homebrew/bin/hashcat
```

5. Navigate to the folder: `cd /opt/homebrew/bin`
6. Move or copy `rockyou.txt` to the current directory:

```bash
cp ~/Downloads/rockyou.txt /opt/homebrew/bin/
```

7. Create a `hash.txt` file in the same directory to store all the hashes that are going to be cracked.

### Task 1 - Cracking the Hashes:

1. **MD5 Hash:**
   - **Hash:** `48bb6e862e54f2a795ffc4e541caed4d`
   - **Mode:** `-m 0` for MD5
   - **Command:**

    ```bash
    ./hashcat -m 0 -a 0 hash.txt rockyou.txt --force
    ```
    Explanation:
    - `-m 0`: Specifies MD5 hash (mode 0).
    - `-a 0`: Dictionary attack mode.
    - `hash.txt`: File containing the hash.
    - `rockyou.txt`: Wordlist to be used.
    - `--force`: Bypass any warnings (especially useful on macOS M1/M2 chips).

    The above command gives result that looks as below:

    ```bash
    ​​Dictionary cache built:
    * Filename..: rockyou.txt
    * Passwords.: 14344391
    * Bytes.....: 139921497
    * Keyspace..: 14344384
    * Runtime...: 1 sec

    48bb6e862e54f2a795ffc4e541caed4d:easy                	 
                                                            
    Session..........: hashcat
    Status...........: Cracked
    Hash.Mode........: 0 (MD5)
    Hash.Target......: 48bb6e862e54f2a795ffc4e541caed4d
    Time.Started.....: Fri Apr  4 11:20:38 2025, (0 secs)
    Time.Estimated...: Fri Apr  4 11:20:38 2025, (0 secs)
    Kernel.Feature...: Pure Kernel
    Guess.Base.......: File (rockyou.txt)
    Guess.Queue......: 1/1 (100.00%)
    Speed.#1.........: 65352.2 kH/s (5.34ms) @ Accel:1024 Loops:1 Thr:64 Vec:1
    Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
    Progress.........: 1048576/14344384 (7.31%)
    Rejected.........: 0/1048576 (0.00%)
    Restore.Point....: 0/14344384 (0.00%)
    Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
    Candidate.Engine.: Device Generator
    Candidates.#1....: 123456 -> Leopold
    Hardware.Mon.SMC.: Fan0: 0%, Fan1: 0%
    Hardware.Mon.#1..: Util: 80%

    Started: Fri Apr  4 11:20:29 2025
    Stopped: Fri Apr  4 11:20:39 2025

    ```
    - **Answer:** `easy`

2. **SHA1 Hash:**
   - **Hash:** `CBFDAC6008F9CAB4083784CBD1874F76618D2A97`
   - **Mode:** `-m 100` for SHA1
   - **Command:**

    ```bash
    ./hashcat -m 100 -a 0 hash.txt rockyou.txt --force
    ```

   - **Answer:** `password123`

3. **SHA256 Hash:**
   - **Hash:** `1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032`
   - **Mode:** `-m 1400` for SHA256
   - **Command:**

    ```bash
    ./hashcat -m 1400 -a 0 hash.txt rockyou.txt --force
    ```

   - **Answer:** `letmein`

4. **Bcrypt-Blowfish Hash:**
   - **Hash:** `$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom`
   - **Mode:** `-m 3200` for bcrypt
   - **Command:**

    ```bash
    ./hashcat -m 3200 -a 0 hash.txt rockyou.txt --force
    ```

   - **Answer:** `bleh`

5. **MD4 Hash:**
   - **Hash:** `279412f945939ba78ce0758d3fd83daa`
   - **Mode:** `-m 900` for MD4
   - **Command:**

    ```bash
    ./hashcat -m 900 -a 0 hash.txt rockyou.txt --force
    ```

   - **Answer:** `Eternity22`

---

## Task 2

1. **SHA256 Hash:**
   - **Hash:** `F09EDCB1FCEFC6DFB23DC3505A882655FF77375ED8AA2D1C13F640FCCC2D0C85`
   - **Mode:** `-m 1400` for SHA256
   - **Command:**

    ```bash
    ./hashcat -m 1400 -a 0 hash.txt rockyou.txt --force
    ```

   - **Answer:** `paule`

2. **NTLM Hash:**
   - **Hash:** `1DFECA0C002AE40B8619ECF94819CC1B`
   - **Mode:** `-m 1000` for NTLM
   - **Command:**

    ```bash
    ./hashcat -m 1000 -a 0 hash.txt rockyou.txt --force
    ```

   - **Answer:** `n63umy8lkf4i`

3. **SHA512crypt Hash:**
   - **Hash:** `$6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.`
   - **Salt:** `aReallyHardSalt`
   - **Mode:** `-m 1800` for SHA512crypt
   - **Command:**

    ```bash
    ./hashcat -m 1800 -a 0 hash.txt rockyou.txt --force
    ```

   - **Answer:** `waka99`

4. **SHA1 with Salt Hash:**
   - **Hash:** `e5d8870e5bdd26602cab8dbe07a942c8669e56d6`
   - **Salt:** `tryhackme`
   - **Mode:** `-m 110` for SHA1 with salt
   - **Command:**

    ```bash
    ./hashcat -m 110 -a 0 hash.txt rockyou.txt --force
    ```

   - **Answer:** `481616481616`
