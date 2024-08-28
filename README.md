# SnowCrash - README

## Overview

The **SnowCrash Project** is a security challenge that requires participants to break into progressively difficult accounts on a virtual machine (VM). Each level presents a unique challenge, and solving it provides the password to the next level. Your objective is to successfully break through all the levels, ultimately reaching the highest level.

You will start on **level00** and proceed by escalating privileges to access **flagXX** accounts, obtaining the password for the next level by executing the `getflag` command.

### Key Points:
- This project will be **evaluated by humans**, so you will need to be able to **explain and demonstrate** your methods and results.
- You will need to use a **64-bit VM** with the provided ISO for this project.
- Each level requires finding a way to exploit system vulnerabilities or perform specific actions to retrieve a password or flag.

## Requirements

1. **Virtual Machine Setup**:
   - Use the provided 64-bit VM ISO to start your machine.
   - Once booted, you will be presented with a prompt displaying the IP address (recommended to use bridge adapater in the VM settings) . If no IP is shown, use the `ifconfig` command to retrieve it.
   
2. **Logging in**:
   - Log into the VM using the following credentials for level00:
     ```
     Login: level00
     Password: level00
     ```
   - It is recommended to connect via **SSH** using port 4242 (please do it):
     ```
     $ ssh level00@<IP_ADDRESS> -p 4242
     ```

3. **Navigating the Levels**:
   - Your task is to find the password for each **flagXX** account, where `XX` is the current level number.
   - After logging in to the **flagXX** account, run the `getflag` command to retrieve the token for the next level.

4. **Session Example**:
   ```bash
   level00@SnowCrash:~$ su flag00
   Password:
   flag00@SnowCrash:~$ getflag
   Check flag. Here is your token: ????????????????
   flag00@SnowCrash:~$ su level01
   Password:
   level01@SnowCrash:~$
   ```

## External Tools

- For some levels, you may need to use external tools or techniques, such as command injection or file manipulation.
- Learn to use the **SCP (Secure Copy Protocol)** command to transfer files between your local machine and the VM:
  ```bash
  $ scp -P 4242 level00@<IP_ADDRESS>:<remote_file> <local_destination>
  ```

## Temporary Directories

- The `/tmp/` and `/var/tmp/` directories have **limited permissions** and are **reset periodically**. Avoid working directly in these directories for extended periods.

## General Tips

- **Attention to Detail**: Each challenge is carefully designed. If something doesn’t seem to be working, double-check your logic and code. Most problems arise from incorrect assumptions.
- **Documentation**: Keep a log of the steps you take to solve each level. This will help you explain your solutions during the evaluation and troubleshoot issues.
- **External Resources**: The challenges may require knowledge of system commands, network utilities, and security vulnerabilities. Research as needed, but don’t rely solely on external help—focus on developing your understanding.
- **Collaboration**: If you encounter issues, don't hesitate to reach out to the educational team or post your questions on the forum, Jabber, IRC, or Slack.
- **Bug Reporting**: In case you encounter a real bug in the VM or the challenges, contact the educational team immediately.

## Evaluation

During your evaluation, you may be required to **demonstrate** how you solved each level and **prove** your results. Be prepared to:
- Show your step-by-step process.
- Explain your reasoning and the tools or commands you used.
- Answer questions about specific vulnerabilities or techniques.

### Good Luck & Have Fun!

---

**Disclaimer**: Always follow ethical guidelines while performing security research. Only attempt exploits or vulnerabilities on systems you are authorized to access, such as this controlled project environment.

