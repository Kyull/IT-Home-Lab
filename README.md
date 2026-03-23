# IT Home Lab

Home lab projects for learning IT, networking, and cybersecurity.

---

## Lab 1: Windows Virtual Machine Setup

### Goal

Create a Windows virtual machine to practice IT troubleshooting and networking tools.

### Tools Used
- Hyper-V
- Windows 10 ISO

### Steps
1. Installed Hyper-V on Windows 11 Pro host
2. Created an External Virtual Switch
3. Created Windows 10 virtual machine with:
   - 6GB RAM
   - 2 CPU cores
   - 40GB dynamically expanding disk
4. Installed Windows 10 inside the VM
5. Tested internet connectivity using `ping google.com`

### Commands Practiced

- `ipconfig`
- `ping google.com` 
- `tracert google.com`
- `netstat -ano`
- `tasklist`

### Skills Learned
- Virtualization basics
- Basic networking diagnostics
- Windows command line tools

---

## Lab 2: Basic Network Diagnostics

### Goal
Practice basic network troubleshooting commands used by IT support technicians.

### Commands Used 

- `ipconfig`
- `ping`
- `tracert`
- `netstat`
- `tasklist`

### Tests Performed

1. Checked network configuration using ipconfig
2. Verified local network connectivity by pinging the default gateway
3. Tested internet connectivity using ping 8.8.8.8
4. Verified DNS resolution by pinging google.com
5. Traced network route using tracert
6. Viewed active network connections with netstat
7. Listed running processes with tasklist

### Skill Learned

- Network connectivity testing
- DNS verification
- Route tracing
- Identifying active network connections

## Lab 3: Windows User Management

### Goal
Practice creating and managing local user accounts using command line tools.

### Commands Used

- `whoami`
- `net user`
- `net localgroup`

### Tasks Performed

1. Checked current user with whoami
2. Listed existing users with net user
3. Created a new user account
4. Added the user to the administrators group
5. Verified group membership
6. Removed the test user account

### Skills Learned

- Managing local user accounts
- Assigning administrator privileges
- Using command line user management tools

---

## Lab 4: Linux Mint Virtual Machine Setup

### Goal

Install a Linux VM to practice Linux command-line and networking basics.

### Tools Used

- Hyper-V
- Linux Mint ISO

### Steps
1. Created Linux VM in Hyper-V
2. Assigned 4GB RRAM and 2 CPU cores
3. Disabled Secure Boot to allow Linux boot
4. Installed Linux Mint
5. Verified network connectivity using ping

### Commands Practiced

- `whoami`
- `pwd`
- `ls`
- `ip a`
- `ping google.com`

### Skills Learned

- Linux installation in a VM
- Basic Linux command-line navigation
- Network interface inspection

---

## Lab 5: Network Communication Between Virtual Machines

### Goal Verify network communication between Windows and Linux virtual machines and troubleshoot connectivity issues.

### Tools Used

```
- ipconfig
- ip a
- ping
```

### Step Performed

1. Identified the Linux VM IP address using:

```
-ip a
```

2. Identified the Windows VM IP address using:

```
- ipconfig
```

3. Attempted to ping the Windows VM from Linux

```
ping 192.168.1.36
```

4. The ping failed with **100% packet loss**, indicating the machines could not communicate.

5. Verified both machines were on the same subnet, confirming basic network configuration was correct.

6. Suspected Windows Firewall was blocking ICMP traffic.

7. Opened **Windws Defender Firewall with Advanced Security**.

8. Navigated to **Inbound Rules** and located:

```
File and Printer Sharing (Echo Request - ICMPv4-In)
```

9. Enabled the rule for the **Private** network profile.

10. Retested connectivity from linux.

11. Successfully received replies, confirming network communication between VM's.

### Result
Both Virtual machines were able to successfully communicate over the network after enabling ICMP echo requests in Windows Firewall.

### Skills Learned
- Identifying IP addresses on Windows and Linux systems
- Testing connectivity using `ping`
- Recognizing firewall related network issues
- Modifying Windows Firewall rules to allow ICMP traffic
- Basic network troubleshooting methodology

---

## Lab 6: Port Scanning and Firewall Behavior

### Goal
Use Nmap to scan a Windows virtual machine from a Linux system in order to identify open ports and understand how firewall rules affect network scanning.

### Tools Used
- Hyper-V
- Windows 10 Virtual Machine
- Linux Mint Virtual Machine
- Nmap

### Commands Used
```
sudo apt update
sudo apt install nmap
nmap -Pn <target-ip>
```

### Steps Performed

1. Installed Nmap on the Linux VM using:
```
sudo apt update
sudo apt install nmap
```

2. Identified the Windows VM IP address using:
```
ipconfig
```

3. Ran an initial scan against the Windows VM:
```
nmap -Pn 192.168.1.36
```

4. The scan returned the result:
```
Host is up.
All 1000 scanned ports are in ignored states.
Not shown: 1000 filtered tcp ports (no-response)
```

5. Determined that the Windows Firewall was blocking inbound scan probes, causing all ports to appear filtered.

6. Enabled **Remote Desktop** in Windows settings to expose a known service.

7. Remote Desktop automatically opened port **3389** in the Windows firewall.

8. Re-ran the Nmap scan from the Linux VM:
```
nmap -Pn 192.168.1.36
```

9. The scan detected an open port:

```
PORT     STATE SERVICE
3389/tcp open  ms-wbt-server
```

10. Confirmed that enabling a service on Windows exposed a network port detectable through Nmap.

### Result
The Nmap scan successfully detected port **3389** after enabling Remote Desktop. This demonstrated how firewall rules and system services determine which ports are visible to other machines on the network.

### Skills Learned
- Installing and using Nmap for network scanning
- Understanding how services expose network ports
- Interpreting filtered vs open ports in scan results
- Observing how firewall rules affect network visibility
- Performing iterative troubleshooting during network analysis

---

## Lab 7: Mapping Open Ports to Windows Processes

### Goal
Identify which Windows processes are responsible for open network ports discovered through scanning.

### Tools Used
- Windows Command Prompt
- netstat
- tasklist
- Nmap

### Commands Used
```
nmap -Pn <target-ip>
netstat -ano
tasklist | findstr <PID>
```

### Steps Performed
1. Confirmed port 3389 was open using Nmap from the Linux VM.
2. Used `netstat -ano` on the Windows VM to identify listening ports.
3. Located port 3389 and recorded the associated PID.
4. Used `tasklist` to identify the process tied to that PID.

### Result
Port 3389 was mapped to the Windows Remote Desktop service running under a system process.

### Skills Learned
- Identifying listening ports on Windows
- Mapping ports to process IDs
- Tracing network services to system processes
- Understanding how services expose ports
   
---

## Lab 8: Network Device Discovery and Identification

### Goal
Use network discovery techniques to identify devices on the local network and investigate device identities using MAC address information.

### Tools Used
- Linux Mint VM
- Nmap
- Linux `ip` networking utilities
- MAC OUI vendor lookup

### Commands Used
```
ip a
nmap -sn 192.168.1.0/24
ip neigh
```

### Steps Performed

1. Identified the local network configuration using:

```
ip a
```

This showed the Linux VM address:

```
192.168.1.38/24
```

From this information the network range was determined to be:

```
192.168.1.0/24
```

2. Performed a network discovery scan using Nmap:

```
nmap -sn 192.168.1.0/24
```

This scan identifies which hosts are active on the local subnet without scanning ports.

3. Observed several responding hosts on the network, including the Linux VM and other devices connected to the home network.

4. Noted that most devices only appeared as:

```
Host is up
```

with little additional information.

5. Used the Linux neighbor table to inspect MAC addresses of discovered devices:

```
ip neigh
```

This command revealed IP-to-MAC address mappings for devices recently communicated with on the network.

Example output:

```
192.168.1.1 dev eth0 lladdr xx:xx:xx:xx:xx:xx REACHABLE
192.168.1.36 dev eth0 lladdr xx:xx:xx:xx:xx:xx STALE
```

6. Extracted MAC addresses and used an OUI vendor lookup database to determine the manufacturer associated with each device.

7. Successfully identified one device on the network as a **TiVo** device based on the registered MAC vendor.

### Result
The network scan successfully identified active devices on the local subnet. Additional analysis of MAC addresses allowed identification of device manufacturers and revealed the presence of a TiVo device on the network.

### Skills Learned
- Determining subnet ranges from CIDR notation
- Performing host discovery scans with Nmap
- Inspecting ARP/neighbor tables in Linux
- Mapping IP addresses to MAC addresses
- Using OUI lookups to identify device manufacturers
- Basic network enumeration techniques

---

## Lab 9: Packet Capture and Network Traffic Analysis

### Goal
Use Wireshark to capture and analyze network traffic between virtual machines and observe protocols such as ARP and ICMP in real time.

### Tools Used
- Linux Mint VM
- Wireshark
- Ping utility

### Commands Used
```
sudo apt install wireshark
sudo gpasswd -a $USER wireshark
ping 192.168.1.36
```

### Setup and Troubleshooting

After installing Wireshark, the application initially did not display the network interface (`eth0`) in the capture interface list.

Only remote capture modules such as:

```
ciscodump
sshdump
wifidump
```

were visible.

This occurred because the user account did not have permission to capture packets.

To fix the issue, the user was added to the Wireshark capture group:

```
sudo gpasswd -a $USER wireshark
```

The Linux VM was then restarted to apply the new permissions.

After rebooting, Wireshark correctly displayed the `eth0` network interface and packet capture was able to begin.

### Steps Performed

1. Installed Wireshark on the Linux VM.
2. Resolved permission issues preventing access to the network interface.
3. Restarted the system to apply the permission changes.
4. Opened Wireshark and began capturing packets on interface:

```
eth0
```

5. Generated network traffic by pinging the Windows VM:

```
ping 192.168.1.36
```

6. Applied display filters in Wireshark to observe specific traffic types:

```
icmp or arp
```

### Observations

During the capture the following network activity was observed:

**ICMP traffic**

The Linux VM sent ICMP Echo Requests to the Windows VM:

```
192.168.1.38 → 192.168.1.36
ICMP Echo Request
```

The Windows VM responded with:

```
192.168.1.36 → 192.168.1.38
ICMP Echo Reply
```

This confirmed successful communication between the two virtual machines.

**ARP traffic**

After the ICMP communication ended, repeated ARP requests were observed from the router:

```
192.168.1.1 → Broadcast
Who has 192.168.1.38?
```

The Linux VM responded with ARP replies confirming its MAC address.

This exchange occurred multiple times as the router refreshed its ARP table to maintain accurate IP-to-MAC mappings on the network.

### Result
Wireshark successfully captured live network traffic between devices on the local subnet. The packet capture demonstrated how ICMP is used for connectivity testing and how ARP is used by devices on the network to resolve IP addresses to MAC addresses.

### Skills Learned
- Installing and configuring Wireshark
- Troubleshooting network capture permissions
- Capturing packets on a network interface
- Filtering packets in Wireshark
- Identifying ICMP communication
- Observing ARP requests and replies
- Understanding how devices discover each other on a local network

---

## Lab 10: Observing a TCP SYN Port Scan with Wireshark

### Goal
Observe how a TCP SYN scan works by capturing Nmap scanning traffic in Wireshark and analyzing the TCP handshake behavior.

### Tools Used
- Linux Mint VM
- Windows 10 VM
- Nmap
- Wireshark

### Commands Used
```
nmap -p 3389 192.168.1.36
nmap -p 3389 -Pn 192.168.1.36
```

### Setup

Packet capture was started in Wireshark on the Linux VM using the network interface:

```
eth0
```

A display filter was applied to isolate TCP traffic:

```
tcp
```

### Steps Performed

1. Started a packet capture in Wireshark on the Linux VM.
2. Initiated a port scan from the Linux VM targeting the Windows VM:

```
nmap -p 3389 192.168.1.36
```

3. Observed that the expected SYN-ACK response was not visible in the packet capture.

4. Determined that Windows firewall behavior and host discovery probes could interfere with the scan.

5. Re-ran the scan using the `-Pn` option to skip host discovery:

```
nmap -p 3389 -Pn 192.168.1.36
```

6. Captured the TCP packet exchange between the Linux and Windows VMs.

### Observations

Wireshark displayed the following packet sequence:

```
Linux VM → Windows VM
TCP SYN
```

```
Windows VM → Linux VM
TCP SYN, ACK
```

```
Linux VM → Windows VM
TCP RST
```

This pattern indicates that the scanned port responded as open.

The final `RST` packet is sent intentionally by Nmap to terminate the connection before the full TCP handshake completes.

### Explanation

A normal TCP connection uses a three-way handshake:

```
SYN → SYN-ACK → ACK
```

However, Nmap performs a **SYN scan**, which works as follows:

```
SYN → SYN-ACK → RST
```

This technique allows the scanner to determine whether a port is open without establishing a full connection.

### Result

The packet capture successfully demonstrated how a SYN scan identifies open ports by analyzing TCP responses.

### Skills Learned

- Performing targeted port scans
- Using Wireshark to observe TCP handshake behavior
- Understanding SYN scanning techniques
- Identifying open vs filtered ports
- Troubleshooting Nmap scans with the `-Pn` option

---

## Lab 11: OS Fingerprinting with Nmap

### Goal
Attempt to identify the operating system of a target machine using Nmap OS detection.

### Tools Used
- Linux Mint VM
- Windows 10 VM
- Nmap
- Wireshark

### Commands Used
```
sudo nmap -O 192.168.1.36
```

### Steps Performed

1. Started packet capture in Wireshark on the Linux VM.
2. Ran an Nmap OS detection scan targeting the Windows VM.
3. Observed scan traffic and responses in Wireshark.
4. Reviewed Nmap output for OS fingerprint results.

### Observations

The scan detected the following open port:

```
3389/tcp open ms-wbt-server
```

This corresponds to the Remote Desktop service.

Nmap reported that OS fingerprinting was unreliable because the scan did not find both an open and closed port. Most ports were filtered by the Windows firewall.

A partial OS guess was produced:

```
Running (JUST GUESSING): Microsoft Windows XP (86%)
```

This occurred because the firewall prevented Nmap from collecting a full fingerprint.

The MAC address of the system was also identified:

```
00:15:5D:01:0C:00 (Microsoft)
```

This indicates the system is running on a Hyper-V virtual network adapter.

### Result

The scan successfully identified the Remote Desktop service but OS detection was limited due to firewall filtering.

### Skills Learned

- OS fingerprinting techniques
- Interpreting Nmap scan results
- Understanding firewall impact on scanning
- Observing reconnaissance traffic in Wireshark


---

## Lab 12: Network Inventory and Device Enumeration

### Goal
Discover devices on the local network and identify active services running on those devices.

### Tools Used
- Linux Mint VM
- Nmap
- Wireshark

### Commands Used
```
nmap -sn 192.168.1.0/24
nmap -Pn <target-ip>
nmap -sV -Pn <target-ip>
```

### Steps Performed

1. Performed a network discovery scan:

```
nmap -sn 192.168.1.0/24
```

This identified active hosts on the local network.

2. Opened Wireshark and applied the following filter to observe discovery traffic:

```
arp
```

3. Observed ARP requests from the Linux VM scanning the subnet:

```
Who has 192.168.1.x?
```

and replies from devices that exist on the network.

4. Used Wireshark statistics to view responding devices:

```
Statistics → Endpoints → IPv4
```

This provided a list of all IP addresses observed during the scan.

5. Selected a device on the network that showed high activity (excluding the router).

6. Performed a port scan on the selected device:

```
nmap -Pn <target-ip>
```

7. Observed multiple open ports on the device, including:

```
62078/tcp open iphone-sync
```

8. Ran service detection for additional detail:

```
nmap -sV -Pn <target-ip>
```

### Observations

- Multiple devices were identified on the local network using ARP-based discovery.
- Wireshark confirmed that Nmap uses ARP requests to identify active hosts on the LAN.
- The selected device exposed several open ports.
- One port was identified as:

```
iphone-sync
```

which is associated with Apple devices such as iPhones or iPads.

- No MAC address was shown in Nmap output for this device, but it was still identifiable based on service fingerprinting.

### Analysis

Open ports indicate that services are actively listening on the device.

However, open ports do not necessarily mean the device is vulnerable. Most modern devices protect these services with authentication and encryption.

This demonstrates the concept of **attack surface**, where each exposed service represents a potential entry point that must be secured.

### Result

Successfully discovered devices on the network and identified active services on a target device.

### Skills Learned

- Network host discovery using Nmap
- Observing ARP-based scanning in Wireshark
- Using Wireshark statistics to identify active hosts
- Performing port scans and service detection
- Interpreting open ports and exposed services
- Understanding attack surface and network exposure

---

## Lab 13: Capturing and Analyzing HTTP Traffic

### Goal
Capture and analyze HTTP web traffic to understand how web requests and responses work.

### Tools Used
- Linux Mint VM
- Wireshark
- Web Browser

### Setup
Started a packet capture on the active network interface (eth0).

Applied Wireshark display filter:
```
http
```

### Steps Performed
1. Opened Wireshark and started capturing on eth0.
2. Opened a web browser and navigated to:
```
http://example.com
```
3. Observed HTTP packets appearing in Wireshark.
4. Clicked on a packet and expanded the Hypertext Transfer Protocol section.
5. Right-clicked the packet and selected:
```
Follow → TCP Stream
```
6. Viewed the full HTTP request and response conversation.

### Observations

HTTP Request:
```
GET / HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
```

HTTP Response:
```
HTTP/1.1 200 OK
Content-Type: text/html
```

The server responded with HTML content used to render the webpage.

### Troubleshooting Notes
- Some requests did not appear because the browser loaded the page from cache.
- Visiting a new site or using Ctrl+Shift+R forces a new request.

### Result
Successfully captured and analyzed HTTP traffic and observed the request/response model.

### Skills Learned
- Using Wireshark display filters
- Following TCP streams
- Understanding HTTP request/response structure
- Understanding browser caching behavior

---

## Lab 14: Intercepting and Modifying HTTP Requests with Burp Suite

### Goal
Intercept and modify HTTP requests using Burp Suite.

### Tools Used
- Burp Suite Community Edition
- Firefox Browser
- Linux Mint VM

### Setup
Configured Firefox proxy settings:
```
HTTP Proxy: 127.0.0.1
Port: 8080
Also use this proxy for HTTPS: enabled
```

Started Burp Suite with:
```
Temporary Project → Use Burp Defaults
```

Enabled interception:
```
Proxy → Intercept → ON
```

### Steps Performed
1. Visited:
```
http://neverssl.com
```
2. Observed browser request intercepted in Burp.
3. Inspected intercepted request:
```
GET / HTTP/1.1
Host: neverssl.com
```
4. Forwarded the request to load the page.
5. Intercepted another request and modified:
```
GET / HTTP/1.1
```
to:
```
GET /test HTTP/1.1
```
6. Forwarded modified request and received a 404 response.

### Troubleshooting Notes
- Some pages did not intercept because they were loaded from browser cache.
- Using a new site or forcing refresh (Ctrl+Shift+R) fixes this.
- Intercepting HTTPS initially caused certificate warnings.
- Installed Burp CA certificate in Firefox to allow HTTPS interception.

### Result
Successfully intercepted and modified HTTP requests using Burp Suite.

### Skills Learned
- Configuring a proxy in Firefox
- Intercepting web traffic
- Modifying HTTP requests
- Understanding HTTPS interception and certificates

---

## Lab 15: Intercepting and Modifying POST Requests

### Goal
Capture and modify POST requests to understand how form data is transmitted.

### Tools Used
- Burp Suite
- Firefox
- http://httpbin.org/forms/post

### Steps Performed
1. Turned Intercept OFF and loaded:
```
http://httpbin.org/forms/post
```
2. Turned Intercept ON.
3. Filled out form fields and submitted.
4. Captured POST request in Burp:
```
POST /post HTTP/1.1

custname=test&custtel=1234
```
5. Modified parameters in Burp before forwarding.
6. Observed server response showing modified values.

### Observations
- Form data is sent in the POST body.
- The server response reflected modified input values.
- This demonstrates that user input can be manipulated before reaching the server.

### Result
Successfully intercepted and modified POST request data.

### Skills Learned
- Understanding POST requests
- Viewing and modifying form parameters
- Understanding how user input is transmitted to servers

---

## Lab 16: Using Burp Repeater to Modify and Replay Requests

### Goal
Use Burp Repeater to resend and modify HTTP requests multiple times.

### Tools Used
- Burp Suite Repeater
- http://httpbin.org

### Steps Performed
1. Captured POST request using Burp Proxy.
2. Right-clicked request and selected:
```
Send to Repeater
```
3. Opened Repeater tab.
4. Modified POST parameters:
```
custname=admin
custtel=wrong
```
5. Clicked Send and observed server response.
6. Repeated with different inputs:
```
custname=admin
custtel=admin

custname=admin
custtel=' OR 1=1 --
```

### Observations
- The server responded differently depending on input values.
- Repeater allows fast testing without resubmitting forms in the browser.
- Multiple repeater tabs were opened accidentally due to Burp UI behavior.

### Troubleshooting Notes
- Burp Repeater UI grouped multiple tabs and displayed a counter (e.g., "6x").
- Closing tabs sometimes increased the counter due to Burp creating new request states.
- Restarting Burp resets Repeater workspace.

### Result
Successfully used Burp Repeater to modify and resend HTTP requests.

### Skills Learned
- Using Burp Repeater
- Modifying request parameters
- Observing server responses
- Understanding how testers manipulate requests

---

## Lab 17: Simulating Login Requests and Testing Input Manipulation

### Goal
Understand how login forms send credentials to a server and how modifying request parameters can affect server behavior.

### Tools Used
- Burp Suite
- Firefox
- http://httpbin.org/forms/post

### Steps Performed

1. Turned Burp Intercept OFF and navigated to:
```
http://httpbin.org/forms/post
```

2. Turned Burp Intercept ON.

3. Filled out the form using test values and submitted the form.

4. Captured the POST request in Burp Proxy.

Example captured request body:
```
custname=admin&custtel=1234&custemail=&delivery=&comments=
```

5. Sent the request to Burp Repeater.

6. Modified the POST parameter to test input manipulation:

```
custname=admin&custtel=' OR 1=1-- 
```

7. Sent modified requests using Repeater and observed server responses.

### Observations

- Form data is transmitted in the HTTP POST body.
- Parameters can be modified before reaching the server.
- The server response reflected whatever values were sent in the request.
- The test site did not perform authentication, but demonstrated how input is transmitted and modified.

### SQL Injection Concept Learned

Learned how SQL injection works conceptually:

Example vulnerable SQL query:
```
SELECT * FROM users
WHERE username = 'admin'
AND password = 'USER_INPUT';
```

Injection input:
```
' OR 1=1-- 
```

Resulting SQL query:
```
SELECT * FROM users
WHERE username = 'admin'
AND password = '' OR 1=1-- ';
```

This makes the WHERE clause always true, which could allow login without a valid password on a vulnerable system.

### Skills Learned

- Intercepting POST requests
- Identifying login parameters
- Modifying request parameters
- Using Burp Repeater to resend modified requests
- Understanding how SQL injection works conceptually
- Understanding how user input reaches backend databases

---

## Lab 18: Cookies and Session Handling

### Goal
Understand how cookies are used to maintain user sessions and how modifying cookies affects server behavior.

### Tools Used
- Burp Suite
- Firefox
- http://httpbin.org/cookies

### Steps Performed

1. Enabled Burp Intercept and navigated to:
```
http://httpbin.org/cookies
```

2. Observed HTTP request headers in Burp.

3. Located the Cookie header in the HTTP request.

Example request:
```
GET /cookies HTTP/1.1
Host: httpbin.org
Cookie: session=12345
```

4. Sent the request to Burp Repeater.

5. Modified the Cookie header manually:

```
Cookie: admin=true
```

6. Sent modified request and observed server response.

7. Tested additional cookie values:
```
Cookie: role=admin
Cookie: loggedIn=true
Cookie: user_id=1
```

### Observations

- The server accepted any cookie value sent by the client.
- The server response reflected the cookie values sent in the request.
- Cookie headers can be modified and resent using Burp Repeater.
- Header order did not affect server response.

### Concepts Learned

- Cookies are used to maintain session state after login.
- The session cookie represents the user's authenticated session.
- If a session cookie is stolen, an attacker may be able to impersonate the user.
- Servers must validate sessions securely and not trust user-controlled cookie values.

### Skills Learned

- Identifying cookies in HTTP requests
- Modifying cookie values
- Using Burp Repeater to test session behavior
- Understanding session-based authentication

---

## Lab 19: IDOR (Insecure Direct Object Reference)

### Goal
Understand how changing object identifiers in HTTP requests can expose unauthorized data.

### Tools Used
- Burp Suite
- Firefox
- https://jsonplaceholder.typicode.com

### Steps Performed

1. Enabled Burp Intercept.
2. Navigated to:
```
https://jsonplaceholder.typicode.com/users/1
```

3. Captured the request in Burp:
```
GET /users/1 HTTP/1.1
Host: jsonplaceholder.typicode.com
```

4. Sent the request to Burp Repeater.
5. Modified the request path from:
```
/users/1
```
to:
```
/users/2
```

6. Sent the modified request.

### Observations

- The server returned a different user's data when the ID number was changed.
- No authentication or authorization check was required to access different user records.
- The server responded successfully to all user ID values tested.

### Concept Learned

This demonstrates an Insecure Direct Object Reference (IDOR), where an attacker can access unauthorized data by modifying object identifiers in a request.

Example vulnerable request:
```
GET /api/user/123
```

Changing the ID:
```
GET /api/user/124
```

If the server does not verify ownership, unauthorized data exposure occurs.

### Skills Learned

- Identifying object IDs in URLs
- Modifying URL parameters in Burp Repeater
- Testing authorization controls
- Understanding IDOR vulnerabilities

