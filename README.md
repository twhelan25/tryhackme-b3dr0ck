![intro](https://github.com/user-attachments/assets/b4dd2b6f-7f4a-4c47-8106-8f6e50a3ca69)

# tryhackme-b3dr0ck

This is a walkthrough for the tryhackme CTF b3dr0ck. I will not provide any flags or passwords as this is intended to be used as a guide. 

## Scanning/Reconnaissance

First off, let's store the target IP as a variable for easy access.

Command: export ip=xx.xx.xx.xx

Next, let's run an nmap scan on the target IP:
```bash
nmap -sV -sC -A -v $ip -oN nmap.txt
```

Command break down:

-sV

Service Version Detection: This option enables version detection, which attempts to determine the version of the software running on open ports. For example, it might identify an HTTP server as Apache with a specific version.
-sC

Default Scripts: This option runs a collection of default NSE (Nmap Scripting Engine) scripts that are commonly useful. These scripts perform various functions like checking for vulnerabilities, gathering additional information, and identifying open services. They’re a good starting point for gathering basic information about a host.
-A

Aggressive Scan: This option enables several scans at once. It combines OS detection (-O), version detection (-sV), script scanning (-sC), and traceroute (--traceroute). It’s useful for a comprehensive scan but can be intrusive and time-consuming.
-v

Verbose Mode: Enables verbose output, which provides more detailed information about the scan’s progress and results.
$ip

Target IP: This is a placeholder for the target IP address you want to scan. In practice, replace $ip with the actual IP of the machine you are targeting.
-oN

Output in Normal Format: This option saves the scan results in a plain text file format. After -oN, specify a filename where you want to store the output.

![nmap](https://github.com/user-attachments/assets/e2558fcf-a2ce-4ac4-b028-8e07c5e533d1)

The scan reveals some interesting ports. Let's check out the webserver on port 80. You will get the warning, so click on anvanced and accept the risk, the page seemed to redirect to port 4040:

![webserver](https://github.com/user-attachments/assets/1fd2fb9f-366a-4d32-8534-28ecd1f420f5)

The fact that we were redirected to 4040 prompted me to run another nmap scan of all the ports:
``` bash
nmap -sV -p- $ip -oN nmap2.txt
```
I also think that Barney was refering to the 9009 port for pichat. The first nmap scan looks like maybe it's ment to be a shell, based on the graphics and strange appearance. Let's try to connect to it via netcat:
