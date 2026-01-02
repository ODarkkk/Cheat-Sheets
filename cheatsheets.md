# CHEATSHEETS

# Access

ssh <user>@<host>
ftp -p <host> <port> <username> <pass> # FTP connection in passive mode. <port>, <username> and <pass> are optional


# Banner Grabbing

netcat <target> <port>
ncat <target> <port>
nc <target> <port>
nc -nv <target> <port>

# Scan

 ## nmap
nmap <target> # Only scan the 1000 most common ports by default.
nmap -sV -sC -p- <target> # Full TCP port scan with service discovery and default scripts
nmap -sC -sV -vv -oA -p- <file> <target> # Full TCP port scan with service/version detection, default scripts, verbose output, and saved results.
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