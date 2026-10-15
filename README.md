
# Man-in-the-Middle Attack on Access Point Management (ARP Spoofing + Credential Capture)

## Project Overview

This lab demonstrates a Man-in-the-Middle (MITM) attack using ARP spoofing to intercept network traffic and capture credentials transmitted through HTTP. The exercise uses a controlled VirtualBox Host-Only network with Kali Linux as the attacker machine and Ubuntu as the victim machine.

The objective is to demonstrate how unencrypted HTTP traffic can expose credentials and highlight the importance of encryption and network security controls.

## Lab Objectives & Tools

- Configure a controlled network environment using VirtualBox.
- Create a simulated router login page.
- Host the login page using a Python HTTP server.
- Verify connectivity between the lab machines.
- Configure IP forwarding and prepare ARP spoofing tools.
- Observe HTTP traffic using Wireshark.
- Analyze an HTTP POST request and identify plaintext credentials.
- Evaluate security measures that help prevent MITM attacks.

**Tools Used:**
- Kali Linux
- Ubuntu Linux
- VirtualBox Host-Only Adapter
- Python HTTP server
- `curl`
- `dsniff`
- Wireshark
- HTML

## Lab Procedure

### 1. Introduction

This lab demonstrates how a Man-in-the-Middle (MITM) attack can be performed using ARP spoofing to intercept network traffic and capture sensitive information. The objective is to show how HTTP transmits credentials in plaintext and why it is insecure.

Two machines were used in a controlled lab environment: a Kali Linux attacker machine and an Ubuntu victim machine, both connected through a Host-Only network.

### 2. Lab Environment

- **Attacker Machine:** Kali Linux
  - IP Address: `192.168.56.102`
  - Interface: `eth0`
- **Victim Machine:** Ubuntu
  - IP Address: `192.168.56.101`
  - Interface: `enp0s3`
- **Network Type:** Host-Only Adapter

### 3. Victim Setup (Fake Login Page)

A fake router login page was created to simulate an access point management interface.

The file was created using nano:

```bash
nano index.html
```
<img width="468" height="39" alt="image" src="https://github.com/user-attachments/assets/6be3a26b-fd4e-4a32-9cfa-424c2b8c9e2c" />

The HTML page contains a login form that sends credentials using an HTTP POST request.

### 4. Web Server Configuration

A Python HTTP server was started on the victim machine to host the login page.

<img width="468" height="302" alt="image" src="https://github.com/user-attachments/assets/4ed15020-719c-4599-bcd4-48fd9f94329c" />

The server listens on port 80 and serves the login page to simulate a real router interface.

### 5. Connectivity Verification

The attacker machine verified access to the victim’s web server using curl.

<img width="468" height="160" alt="image" src="https://github.com/user-attachments/assets/27ac9f0e-dbed-4e7f-95d4-c6e63b9c1aa8" />

This confirmed that the attacker could reach the victim system over the network.

### 6. Attacker Configuration

IP forwarding was enabled on the attacker machine to allow packet routing.

<img width="468" height="110" alt="image" src="https://github.com/user-attachments/assets/18bdd3cc-3258-41d5-9ca6-c52c5f675da2" />
<img width="468" height="60" alt="image" src="https://github.com/user-attachments/assets/84f9611c-91f6-4780-b1c0-1d1fe66b0343" />


### 7. ARP Spoofing Attack

The attacker launched ARP spoofing to position itself between the victim and the gateway.

<img width="468" height="198" alt="image" src="https://github.com/user-attachments/assets/5c4fbaaf-d7f1-4bb5-b325-cbdc54d1fa0a" />

This allowed the attacker to intercept traffic by poisoning ARP tables.

### 8. Victim Login Simulation

The victim accessed the fake login page and entered the following simulated credentials:

- **Username:** `admin`
- **Password:** `password123`

<img width="468" height="150" alt="image" src="https://github.com/user-attachments/assets/b34e9c35-bcb6-4327-8c4f-49f269255ecc" />

### 9. Traffic Capture with Wireshark

Wireshark was used on the attacker machine to capture HTTP traffic.

<img width="468" height="118" alt="image" src="https://github.com/user-attachments/assets/df7d4c32-8f6f-4c9a-82ff-00ba317af7fb" />

A POST request was identified containing the login submission.

### 10. Credential Extraction

The HTTP POST request was analyzed using “Follow TCP Stream” in Wireshark.

<img width="468" height="120" alt="image" src="https://github.com/user-attachments/assets/94845e50-f4e5-4a2d-ae0b-186471beb47e" />

The captured data revealed:

```text
username=admin&password=password123
```

This confirms that credentials were transmitted in plaintext.



## Analysis

The lab demonstrated that:

- The attacker machine could access the victim's hosted login page.
- ARP spoofing was used to position the attacker between network devices.
- Wireshark was used to inspect the HTTP login submission.
- The HTTP POST request exposed the submitted test credentials in plaintext.
- Unencrypted HTTP traffic can expose sensitive information to network observers.

The results illustrate the risks of transmitting credentials over HTTP rather than HTTPS.

## Security Recommendations

- **Enforce HTTPS:** Use encrypted connections to protect credentials and other sensitive information in transit.
- **Replace Default Credentials:** Use strong, unique passwords for network equipment and management interfaces.
- **Monitor ARP Activity:** Investigate unexpected changes in ARP mappings and suspicious traffic.
- **Use Network Protections:** Consider Dynamic ARP Inspection, DHCP snooping, or static ARP entries where appropriate and supported.
- **Segment Networks:** Use VLANs and access controls to limit unnecessary communication between devices.
- **Avoid Real Credentials in Labs:** Use simulated credentials and isolated lab environments for security testing.

## Results & Conclusion

This project provided practical experience with MITM concepts, ARP spoofing, HTTP traffic inspection, and packet analysis using Wireshark. It demonstrated how plaintext credentials can be exposed when encryption is not used and reinforced the importance of secure protocols, network monitoring, and defensive network configuration.
