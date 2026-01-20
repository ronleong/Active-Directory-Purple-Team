# Active-Directory-Attack-Defense-Project

## Objective
The Active Directory Attack & Defense project simulates a complete attack life cycle within an Active Directory enviroment. The main goal was to demonstarted both offensive pentest and defensive secuirty engineering by executing multi stage attack chain then developed and implemnemtn automated SOAR solution to detect and block threat in real time.

---

## Skills Learned
- Privilege Escalation  
- Threat Detection  
- Automated Response  
- Active Directory Exploitation  

---

## Tool Used
- Wazuh  
- Kali linux  
- Oracle Virtual Box  
- Sysmon  
- Window Server2019  
- GodPotato  
- John the Ripper  
- Nmap  
- Impacket Suite  

---

## Key Result

### Offensive Operation
- Successfully compromised the whole Active Directory domain by launching mulitple attack technique  
- Upgraded from low level user to SYSTEM privileges using service account exploitation and Windows token manipulation  
- Created Golden Ticket for complete domian access  

### Defensive Engineering
- SIEM implementation  
- Log Analysis & Detection  
- Automated Incident Response  

---

## Project Walkthrough

---

<details>
<summary><strong>Phase 1: Attack Simulation</strong></summary>

<br>

### Initial Reconnaissance
**Tool:** Responder  

**Action:**  
Analyzed traffic and performed LLMNR poisoning to capture NTLMv2 hashes from domain trying to connect to non-existent file shares  

**Result:**  
Capture encrypted credentials for valid users  

<img width="607" height="454" alt="poision llmnr" src="https://github.com/user-attachments/assets/e6c34e84-ade6-435e-a89f-afd18fba572e" />

---

### Initial Access
**Tool:** Nmap, Impacket  

**Action:**  
Identify an exposed port named 1433. Used compromised credentials to authenticated and enabled `xp_cmdshell` to execuete remote system commands  

**Result:**  
Established an initial shell on the domain server  

<img width="442" height="406" alt="check port and ask what permission db attack" src="https://github.com/user-attachments/assets/e304cf12-561c-47fe-9809-36a1a024a64a" />

---

### Credential Exploiotation
**Tool:** Impacket, John the Ripper  

**Action:**  
Requested a Kerberos Service Ticket for the `sqlsvc` account and extracted the encrypted user's NTLM hash for online cracking  

**Result:**  
Succssfully cracked the user password using John the ripper to reveal plaintext password `Password@123`  

<img width="410" height="432" alt="kerberoasting attack" src="https://github.com/user-attachments/assets/d23a78ed-fe40-4b69-91cf-72681c5a31fa" />

---

### Privilege Escalation
**Tool:** GodPotato  

**Action:**  
Leveragred the `SeImpersonatePrivilege` token and exploit the DCOM call and impersonate SYSTEM account using GodPotato  

**Result:**  
Escalted privilege from low level user to Root (`NT AUTHORITY\SYSTEM`)  

<img width="443" height="253" alt="golden patato into system" src="https://github.com/user-attachments/assets/a85049c6-decf-43c7-a24f-b58c23d067e4" />

---

### Domain Dominance
**Tool:** Impacket  

**Action:**  
Dumped the Active Directory Database to retrieve KRBTGT hash and use it to forge a Kerberos Golden Ticket  

**Result:**  
Gained unrestricted, persistent administrative access to the Domain Controller  

<img width="443" height="253" alt="golden ticket" src="https://github.com/user-attachments/assets/9eb7e99d-b9c8-4871-8801-9247b85195d0" />

</details>

---

<details>
<summary><strong>Phase 2: Detection & Analysis</strong></summary>

<br>

### Log Ingestion and Monitoring
Deployed Wazuh Agents and Sysmon on Domain Controller to foward it to Wazuh Manager  

### Threat Hunting
- **Event ID 4624:** Detected Network Logon lateral movement from attacker IP  
- **Event ID 4672:** Flagged Special Privilege Assigned matched the executio of GodPotato exploitation and Administrator login  
- **Event ID 4688:** Traced the process creation tree identifying `WmiPrvSE.exe` spawning `cmd.exe`  

</details>

---

<details>
<summary><strong>Phase 3: Automated Defense</strong></summary>

<br>

### Set up Wazuh Rule
**Tool:** Wazuh Manager  

**Action:**  
Created a custom rule to trigger the `win_route-null` script whenever a high severity alert matching the `Credential Dumping` rule ID was generated  

**Result:**  
The system now is ready to react for specific threat automatically  

<img width="284" height="121" alt="set up wazuh rule for soc" src="https://github.com/user-attachments/assets/e43ace5d-7a21-4118-8a58-2b2adc565c6b" />

---

### Validation
**Tool:** Kali linux, Window Firewall  

**Action:**  
Re-attempted the `secretsdump.py` attack to test the active defense mechanism  

**Result:**  
The attack failed with a `Connection Refused` error  

<img width="535" height="150" alt="wazuh rule working" src="https://github.com/user-attachments/assets/54da563e-8bc0-4282-82fb-2f3eeba9e172" />

</details>

