**Project: Analysis of phishing email with attachment that leads to malware XYZ that is related to Ransomware**

**Step 1: make sure you have a VM running**

The kali linux is running and I took the fresh installation snapshot to be on the safe side, if the analysis of malware leads to any problem in the OS. so that i can revert back to fresh state.

![](/images/lME_Image_1.png)

**Fresh installation snapshot**

![](/images/yHY_Image_2.png)

**Step 2: upload homework.zip to your VM**

I downloaded the **homework.zip** from the portal on my Windows 10 host machine and then drag and drop to the Kali Virtual machine.

![](/images/kBo_Image_3.png)

After copying the **homework.zip** file on the desktop. I open the **Terminal.**

a.	I run the **PWD** command to check current working directory	.

	**???(ghazi@kali)-[~]**

**??$ pwd**

**/home/ghazi**

b.	I run the **ls** command to see available directories in** /home/ghazi.**

**???(ghazi@kali)-[~]**

**??$ ls**

**Desktop  Documents  Downloads  homework  Music  Pictures  Public  Templates  Videos**

c.	I run the **CD Desktop** command to go inside the **Desktop directory**.

**???(ghazi?kali)-[~]**

**??$ cd Desktop**

d.	I created a directory **assignment **on the desktop.

**???(ghazi@kali)-[~/Desktop]**

**??$ mkdir assignment**

e.	I moved the **homework.zip** file to the **assignment** directory.

**???(ghazi@kali)-[~/Desktop]**

**??$ mv homework assignment**

f.	I changed my current directory from **Desktop** to **assignment**.

	**???(ghaz@kali)-[~/Desktop]**

**??$ cd assignment**

g.	I run the **ls** command to see available files in** /Desktop/assignment**

	**???(ghazi@kali)-[~/Desktop/assignment]**

**??$ ls**

**homework**

h.	I unzip the **homework.zip** file to the **assignment** directory. I provided the required

password and homework,zip was successfully unzipped.

**???(ghaz@kali)-[~/Desktop/assignment]**

**??$ unzip homework**

**Archive:  homework**

**[homework] ****phishing.eml**** password: **

**inflating: phishing.eml**

**inflating: phishing_analysis.py**

**inflating: phishing_analysis2.py**

**inflating: malware_traffic.pcap**


**Step 3: Analyse phishing email with Python script**

As we do have a third party solution** eml_parser module** in **python3** to parse the email but it requires download and dependencies over third party solutions.

I decided to find a way where no third party involvement is required. I did the research by using **ChatGPT** and discovered that there is an inbuilt solution of parsing the email in **Python3** called as **email module**.

**Following are the prompts I used with their respective results.**

[https://docs.google.com/document/d/1jKk-vP2eM_TA7C5cppiQ1d6B6-PmGghYBlVMxtM9Drs/edit?usp=sharing](https://docs.google.com/document/d/1jKk-vP2eM_TA7C5cppiQ1d6B6-PmGghYBlVMxtM9Drs/edit?usp=sharing)

**Script Preparation and editing**

a.	I run the **Nano** command to edit the python script **phishing_analysis.py  **

	**???(ghazi@kali)-[~/Desktop/assignment]**

**??$ nano phishing_analysis.py**

![](/images/IJe_Image_4.png)

b. 	I edited the existing script and deleted all the contents by using combination keys **Ctrl+K**	**  **

C.	I copied the script from ChatGPT and pasted it to the** Nano editor**.	

**import email**

**import hashlib**

**from email import policy**

**from email.parser import BytesParser**

Define the EML file name
**eml_file = "phishing.eml"**

Open and parse the EML file
**with open(eml_file, "rb") as file:**

**msg = BytesParser(policy=policy.default).parse(file)**

Print email headers
**print(f"From: {msg['From']}")**

**print(f"To: {msg['To']}")**

**print(f"Subject: {msg['Subject']}")**

**print(f"Date: {msg['Date']}\n")**

Print plain text body (first match)
**for part in msg.walk():**

**if part.get_content_type() == "text/plain":**

**print("Body:\n" + part.get_content())**

**break**

Process attachments
**for part in msg.iter_attachments():**

**filename = part.get_filename()**

**data = part.get_payload(decode=True)**

**sha256 = hashlib.sha256(data).hexdigest()**

**print("\n--- Attachment Found ---")**

**print(f"Filename: {filename}")**

**print(f"SHA256 Hash: {sha256}")**

D.	I saved the script by pressing **Ctrl+O, Enter**. Then press **Ctrl+x** to exit the **Nano Editor**.

**Running the Script**

I run the command **python3 phishing_analysis.py** to run the newly saved script.

	**???(ghazi@kali)-[~/Desktop/assignment]**

**??$ python3 phishing_analysis.py**

![](/images/XP5_Image_5.png)

![](/images/CYj_Image_6.png)

The script successfully parse the phishing.eml file and provided the details including attached file name which is **STATEMENT OF ACCOUNT.zip** with its **SHA256 hash value** which is **SHA256 Hash: e0b82e45a3f9730b6a4984918864330c7783b90f9a88276d18146587d5657ce8**


**Step 4: Analyze extracted attachment using third party services**

After successfully parsing the email, I need to check the email attachment for any malicious properties.

I visited the [https://www.virustotal.com/gui/home/upload](https://www.virustotal.com/gui/home/upload) and [https://tria.ge/s](https://tria.ge/s) to analyse the hash value. I uploaded the hash value to the website. This hash is flagged as Malicious.

Following are my findings.

 MALWARE STEALER TROJAN EVADER

1. What malware family is this attachment?

**Family label are Agenttesla, Taskun**

2.	What malware type is that?

**Taskun**

This is a **malware family name**. \
**Taskun** trojans are typically **Remote Access Trojans (RATs)** or **info-stealers**. \
Known for using **obfuscation**, persistence, and **spying capabilities**.

**Common Features:**

Keyboard logging \
Screen capture \
Exfiltration of files \
C2 communication

### **AgentTesla**

A **very well-known and widespread malware family**. \
It's a **commercial keylogger and remote access trojan** sold on dark web forums. \
Written in **.NET/MSIL \
**Frequently spread via **malicious email attachments** (e.g., `.doc`, `.exe`, `.zip`). \


**AgentTesla Capabilities:**

Keylogging \
Credential theft (from browsers, email clients, VPNs) \
Clipboard monitoring \
Screenshot capture \
Data exfiltration via SMTP/FTP/HTTP

**Step 5: So what moment?**

**Why is this malware family type relevant to the business?**

The **AgentTesla** and **Taskun** malware families are highly relevant to a business environment due to their **data theft**, **credential compromise**, and **remote access** capabilities ? all of which directly threaten the **confidentiality, integrity, and availability** of corporate assets.

**AgentTesla** can steal credentials from browsers, VPNs, email apps ? risking broader compromise. \
**Taskun** may provide remote desktop access or exfiltrate internal financial documents. \
**Accounting users** often have access to **financial systems**, which can be exploited for fraud or ransomware attacks.

**Step 6: What would be your recommendation?**

**If George from Accountants ended up executing this file on his machine, what should the IT Security team do?**

### **1. Isolate the Infected Machine**

- Disconnect George?s system from the network **immediately** (Wi-Fi, LAN, VPN). \


- Prevent further data exfiltration or lateral movement.

### **? 2. Preserve Evidence**

- Do **not** power off the system yet. \


- Create a memory dump (RAM image) and disk image for forensic analysis.

- Capture logs, running processes, and network activity.

### **? 3. Contain and Scan**

- Run a **full malware scan** using trusted AV/EDR tools. \


- Check for known **AgentTesla** or **Taskun** persistence mechanisms: \


- Registry entries

- Scheduled tasks

- Startup folders

- Clean or reimage the machine based on results.


## **? Investigation and Threat Hunting**

### **? 4. Credential Reset**

- Reset passwords for George?s:

- Email

- VPN

- Business apps

- Windows/AD login

- Investigate credential reuse across systems.

### **? 5. Check for Lateral Movement**

- Review logs from firewalls, EDR, SIEM.

- Look for:

- Unusual access patterns

- Data transfers

- Suspicious logins from George?s account

- Scan other machines in the subnet for infection.

### **?? 6. Detect Communication with C2 Servers**

- Review DNS and proxy logs.

- Use IoCs (Indicator of Compromise) for AgentTesla/Taskun:

- Known domains

- IP addresses

- Hashes of payloads


## **?? Long-Term Security Actions**

### **? 7. Patch and Harden Systems**

- Apply latest security patches to George?s PC and the entire network.

- Disable macros in Office docs.

- Implement application whitelisting.

### **? 8. User Awareness Training**

- Remind staff not to open unsolicited attachments or emails.

- Simulate phishing campaigns to test alertness.

### **? 9. Incident Report & Legal**

- Document the full incident, impact, and response.

- Notify stakeholders if sensitive data was involved.

- If required, report to regulators under data protection laws.


## **? Why This is Critical**

- **AgentTesla** can steal credentials from browsers, VPNs, email apps ? risking broader compromise. \


- **Taskun** may provide remote desktop access or exfiltrate internal financial documents. \


- **Accounting users** often have access to **financial systems**, which can be exploited for fraud or ransomware attacks.

