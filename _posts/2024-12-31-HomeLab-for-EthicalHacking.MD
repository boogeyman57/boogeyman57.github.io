---
layout: "post"
title: "HomeLab-for-EthicalHacking"
categories: [HomeLab]
tags: [HomeLab, Ehical Hacking, Penetration Testing]

---

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*0tsU4WfeeF3-QMwR7f118w.png)

Creating a safe and functional environment to practice ethical hacking is essential for aspiring security professionals. A home lab offers the perfect space to experiment with tools, learn offensive security techniques, and refine your skills without risking real-world systems.

In this blog, I document the process of setting up my ethical hacking home lab, including network configuration, virtual machine setup, and ensuring everything operates securely and efficiently. Follow along to build your own lab and start your journey into cybersecurity.

# Network Overview

The lab is built using **VirtualBox** and a custom **NAT network**, acting as a private network to provide IP addresses to all virtual machines. This ensures isolation and safety while replicating a real-world network environment. Here’s a breakdown of the network:

- **Network Name:** ExampleCorpNet
- **CIDR:** 10.10.10.0/24
- **Machines:** 
  - Kali Linux (Analyst/Attacker Machine)
  - TestLab (Target VM)
  - SBVA (Target VM)

# Steps to Configure the Home Lab

## 1. Setting Up VirtualBox
First, download and install VirtualBox. Then create a **NAT network** with the following settings:
- Go to File > Tools > Network Manager > NAT Networks tab.
- **Name:** ExampleCorpNet
- **CIDR:** 10.10.10.0/24
![](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*WMF2nWbYKGs4L2fWe16PQg.png)
This network allows seamless communication between the machines while keeping them isolated from your host system.

## 2. Adding Virtual Machines
### **Kali Linux (Attacker Machine):**
- Download the pre-built **Kali Linux VirtualBox image** from the official site.
- Import the `.vbox` file into VirtualBox.
- Configure the second network adapter to use the ExampleCorpNet NAT network.
- In VirtualBox, select Machine > Settings > Network > Adapter 2.
- Attach it to the ExampleCorpNet NAT network you created earlier.

### **Target Machines:**
- Download and import the `.ova` files for the **TestLab**(https://www.notion.so/Home-Lab-for-Ethical-Hacking-16c5cfabf4b5804c8627e4a9f4512625?pvs=21) and **SBVA**(https://www.notion.so/Home-Lab-for-Ethical-Hacking-16c5cfabf4b5804c8627e4a9f4512625?pvs=21) machines.
- go to Machine > Settings > Network > Adapter 1
- Assign them to the ExampleCorpNet NAT network for Adapter 1.

## 3. Configuring Hostnames and IPs
To ensure smooth communication between machines, update the hosts file on the Kali Linux machine:


sudo nano /etc/hosts

![](https://miro.medium.com/v2/resize:fit:640/format:webp/1*S2tXy6v0WUqxLE-qgpuldw.png)

10.10.10.10    example.com
10.10.10.15    testlab.example.com testlab.local
10.10.10.101   sbva.local

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*T3jxT4n_DRvdFfzBGtrVNg.png)

## 4. Update kali Linux
After installation, update all tools and packages:

sudp apt update && sudp apt upgrade


# Network Configuration

The NAT network functions as a private network with DHCP capabilities. It assigns IP addresses dynamically, ensuring connectivity between the attacker machine and targets. Here's a simplified flow:

- NAT Network: Routes internal traffic between VMs in the 10.10.10.0/24 subnet.
- Kali Linux: Used for attacking and analyzing target machines.
- TestLab and SBVA: Target VMs configured to respond to simulated attacks.

# Visual Representation
Below is a simple diagram of the lab setup:

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*qRqA4lcBa5Otpbe_2CcZig.png)



# Key Benefits of This Lab

- Isolation: The lab is completely separated from the host system, reducing risk.
- Real-World Simulation: Configurations mimic real-world network setups, providing a practical learning environment.
- Scalability: Easily add more machines or networks as needed for advanced scenarios.


