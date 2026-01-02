# CHEATSHEETS

# Access

ssh <user>@<host>

# Banner Grabbing

netcat <ip> <port>
ncat <ip> <port>
nc <ip> <port>
nc -nv <ip> <port>

# Scan

nmap <ip> # Only scan the 1000 most common ports by default.
nmap -sV -sC -p- <ip> # Full TCP port scan with service discovery and default scripts
nmap -sC -sV -vv -oA -p- <file> <ip> # Full TCP port scan with service/version detection, default scripts, verbose output, and saved results.
nmap --script <script name> -p<port> <host> # Runs a specific NSE script against a chosen port on the target host
nmap -sV --script=banner <target> # Detects running services and retrieves service banners from the target
