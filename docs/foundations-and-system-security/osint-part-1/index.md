# Reconnaissance & OSINT — Part 1
---

> Lab Type: Hands-on Reconnaissance & OSINT (Passive and Active Information Gathering)  
> Tool: WHOIS, Shodan, crt.sh, Subfinder, httpx, Gobuster, Kali Linux Terminal, Web Browser 
> Framework: OSINT Methodology / Ethical Hacking Reconnaissance Phase
> System: Kali Linux (or any Linux distribution), Windows 10/11 (for browser-based tasks)

---

## Lab Objectives
Understand the role of reconnaissance in the ethical hacking lifecycle.
Differentiate between passive and active reconnaissance techniques.
Perform WHOIS lookups to gather domain registration information.
Use OSINT tools to identify exposed assets and enumerate subdomains.

## Milestone 1 - Active vs Passive Reconnaissance
### What is Reconnaissance?
Recon is the first phase of the ethical hacking / pentesting lifecycle (Recon → Scanning →
Gaining Access → Maintaining Access → Covering Tracks). The goal is to gather as much
information as possible about a target before touching it directly.

### Passive Recon
Definition: Gathering information without directly interacting with the target's
systems. The target has no way of knowing they are being investigated.
Sources: WHOIS records, DNS history, search engines, Shodan/Censys, social media,
job postings, leaked data (Have I Been Pwned), Wayback Machine, GitHub repos,
Google Dorking.
Risk to you: Very low — you're querying third-party databases, not the target itself.
Analogy: Reading a company's public annual report and LinkedIn page before a
meeting.

### Active Recon
Definition: Directly interacting with the target's infrastructure — sending packets,
requests, or queries to their systems.
Sources: Nmap port scans, DNS zone transfer attempts, banner grabbing, live
subdomain probing (sending HTTP requests to check if a subdomain resolves),
traceroute.
Risk to you: Higher — this traffic can be logged, trigger IDS/IPS alerts, and is only
legal with explicit authorization.
Analogy: Walking up to the building and testing which doors are unlocked

### Quick Comparison Table
Aspect Passive Recon Active Recon
Target interaction None (uses 3rd-party data) Direct
Detectability Undetectable by target Can trigger logs/alerts
Legal risk Low Requires authorization
Examples WHOIS, Shodan, Google Dorking Nmap scan, live subdomain probing
Speed Fast Can be slower/noisier

## Milestone 2 - WHOIS Lookup (Passive)

### What WHOIS Tells You
WHOIS is a protocol/database that stores registration details of a domain: registrar,
creation/expiry date, name servers, and (if not privacy-protected) registrant contact info.
It's fully passive — you're querying a public registry database, not the target's server.

### Method A: Command Line (Kali/Linux)
Step 1 — Install whois (if not already present):
```sudo apt update```
```sudo apt install whois -y```

Step 2 — Run a WHOIS query:
```whois example.com```

Step 3 — Read the output. Focus on:
Registrar: — who the domain was bought through
Creation Date: / Updated Date: / Registry Expiry Date:
Name Server: — this tells you who hosts DNS (often reveals if they use Cloudflare,
AWS Route53, GoDaddy DNS, etc.)
Domain Status: — e.g., clientTransferProhibited
Registrant Name/Org/Email — often redacted by GDPR privacy proxies nowadays

### Method B: Web-Based WHOIS (no install needed)
Useful as a cross-check or if CLI is unavailable.
1. Go to who.is or whois.domaintools.com
2. Enter the target domain
3. Compare results to your CLI output

### What To Do With This Info
1. Name servers tell you the hosting/DNS provider (useful for later subdomain/DNS
enumeration).
2. Creation date helps assess if it's a long-standing or freshly registered (possibly
suspicious) domain.
3. If registrant info is visible (not privacy-protected), it can reveal organizational
contacts — relevant in social engineering awareness training.

## Milestone 3 - Shodan (Passive — "The Search Engine for Devices")
### What Shodan Does
Shodan continuously scans the entire internet and indexes banners from open
ports/services (web servers, cameras, industrial control systems, databases, routers, etc.).
Instead of you scanning the target directly, you search Shodan's existing index — making
this passive.

### Step 1 — Create an Account
1. Go to https://www.shodan.io
2. Sign up for a free account (free tier gives limited but sufficient results for lab use)
3. Verify your email

### Step 2 — Basic Web Search
In the Shodan search bar, try: hostname:example.com or org:"Example Organization"
What you'll see per result:
IP address
Open ports and the banner/service running on each (e.g., 80/tcp — nginx 1.18.0 )
Geolocation of the server
Last time Shodan scanned it
Sometimes: SSL certificate details, HTTP headers, favicon hash

### Step 3 — Useful Shodan Search Filters
Filter Purpose Example
hostname: Search by domain/hostname hostname:example.com
port: Filter by specific port port:22
hostname:example.com
org:"Example Organization"
Filter Purpose Example
org: Search by organization name org:"Acme Corp"
country: Filter by country code country:IN
product: Filter by software/product product:nginx
net: Search a CIDR/IP range net:192.168.1.0/24

### Interpreting Results Responsibly
1. Open ports + old software versions = potential vulnerability, but do not attempt to
connect/exploit anything you don't own.
2. This is purely for identifying exposure/attack surface in a report, e.g., "Host X exposes
RDP (3389) to the internet, which is a common ransomware entry vector."

## Milestone 4 - Subdomain Enumeration (Passive + Active Hybrid)
Subdomains often run older, forgotten, or staging applications ( dev. , staging. , test. ,
old-portal. ) that are less protected than the main site — making enumeration a key recon
step.

### Passive Method 1 — crt.sh (Certificate Transparency Logs)
SSL certificates are logged publicly. Any subdomain that ever got an HTTPS cert shows up
here — no interaction with the target at all.
1. Go to https://crt.sh/?q=%25.example.com
2. Review the list of subdomains found in certificate records

### Passive Method 2 — Subfinder (Aggregates Many OSINT Sources)
```sudo apt install subfinder -y```

or via go:

```go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest subfinder -d example.com -o subfinder_results.txt```

Subfinder queries dozens of passive sources (crt.sh, VirusTotal, Shodan, etc.) at once and
gives you a clean list.

### Active Method — DNS Resolution / Live Host Check
Once you have a list of candidate subdomains, you actively check which ones actually
resolve/respond (this step does touch the target, so only do this on domains you're
authorized to test):

```cat subfinder_results.txt | httpx -o live_subdomains.txt```

( httpx sends a lightweight HTTP request to each subdomain to see which are alive.)
Install httpx if needed:

```go install -v github.com/projectdiscovery/httpx/cmd/httpx@latest```

### Active Method — Brute-Force with a Wordlist (gobuster)
```sudo apt install gobuster -y && gobuster dns -d example.com -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt```

### Consolidating Results
Combine all outputs into one deduplicated list:
```cat subfinder_results.txt amass_results.txt | sort -u > all_subdomains.txt wc -l all_subdomains.txt```

## Conclusion
This lab introduced the fundamentals of reconnaissance and OSINT by demonstrating both passive and active information-gathering techniques. It provided practical experience with industry-standard tools to identify publicly exposed information and understand an organization's external attack surface while emphasizing ethical and authorized usage.

## Lab Outcomes
Identified differences between passive and active reconnaissance.
Collected domain registration details using WHOIS.
Gathered publicly available information using Shodan and certificate transparency logs.
Enumerated and validated subdomains using passive and active techniques.




