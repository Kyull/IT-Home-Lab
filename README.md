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
