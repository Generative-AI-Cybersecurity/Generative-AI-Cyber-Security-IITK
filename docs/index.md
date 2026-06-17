<style>
.md-sidebar--primary,
.md-sidebar--secondary {
    display: none !important;
}

.md-content {
    max-width: 100% !important;
    margin: 0 !important;
    padding-top: 0 !important;
}

.md-content h1,
.md-path,
.md-breadcrumb {
    display: none !important;
}
</style>

<div class="cyber-home">

<section class="cyber-hero">

<div class="hero-overlay"></div>

<div class="hero-content">

<div style="text-align: center; margin-top: 0px; margin-bottom: 0px;">
  <img src="images\image-1.jpg" style="max-width: 800px; height: auto;">
</div>

<h2 style="color:blue; margin-bottom:10px; font-weight:600;">
Professional Certification Course in Generative AI & Cybersecurity
</h2>

<span class="hero-badge">
EICTA CYBERSECURITY
</span>

<p>
Hands-on labs, cybersecurity exercises, AI-powered security workflows,
and enterprise-grade cybersecurity simulations.
</p>

<div class="hero-buttons">
<a href="foundations-and-system-security/microsoft-threat-model-software/" class="primary-btn">Start Learning</a>
<a href="#labs" class="secondary-btn">Browse Labs</a>
</div>

<div class="hero-stats">
<div>
<span>12+</span>
<p>Lab Modules</p>
</div>

<div>
<span>20+</span>
<p>Security Tools</p>
</div>

<div>
<span>9</span>
<p>Months</p>
</div>

<div>
<span>1</span>
<p>Capstone Project</p>
</div>
</div>

</div>

</section>

<section id="labs" class="labs-section">

<h2>Lab Modules</h2>

<div class="lab-grid">

<a href="foundations-and-system-security/microsoft-threat-model-software/" class="lab-card">
<h3>Foundations and System Security</h3>
<p>Cybersecurity fundamentals and threat landscape</p>
</a>

<a href="pentesting-and-ethical-hacking/coming-soon/" class="lab-card">
<h3>Pentesting and Ethical Hacking</h3>
<p>Protocols, packet analysis and attack surfaces</p>
</a>

<a href="pentesting-and-ethical-hacking/coming-soon/" class="lab-card">
<h3>Advance Red Teaming + AI-Driven Attacks</h3>
<p>Hardening, permissions and administration</p>
</a>

<a href="pentesting-and-ethical-hacking/coming-soon/" class="lab-card">
<h3>SOC, DFIR + AI/ML for Defense</h3>
<p>Reconnaissance, exploitation and reporting</p>
</a>

<a href="pentesting-and-ethical-hacking/coming-soon/" class="lab-card">
<h3>Cloud Security, DevSecOps & Quantum Security</h3>
<p>Advanced adversary emulation</p>
</a>

<a href="pentesting-and-ethical-hacking/coming-soon/" class="lab-card">
<h3>Capstone Project</h3>
<p>Detection, monitoring and response</p>
</a>


</div>

</section>

<!-- <section id="learning-path" class="path-section">

<h2>Learning Path</h2>

<div class="path-container">

<div class="path-step">01 Foundations</div>
<div class="path-line"></div>

<div class="path-step">02 Networking</div>
<div class="path-line"></div>

<div class="path-step">03 Linux Security</div>
<div class="path-line"></div>

<div class="path-step">04 Pentesting</div>
<div class="path-line"></div>

<div class="path-step">05 Red Teaming</div>
<div class="path-line"></div>

<div class="path-step">06 SOC & DFIR</div>
<div class="path-line"></div>

<div class="path-step">07 Cloud Security</div>
<div class="path-line"></div>

<div class="path-step">08 DevSecOps</div>
<div class="path-line"></div>

<div class="path-step">09 AI Security</div>
<div class="path-line"></div>

<div class="path-step">10 Capstone</div>

</div>

</section> -->

<section class="arsenal-section">

<h2>Security Arsenal</h2>

<div class="arsenal-grid">

<div class="arsenal-card">Burp Suite</div>
<div class="arsenal-card">Metasploit</div>
<div class="arsenal-card">Nmap</div>
<div class="arsenal-card">Wireshark</div>
<div class="arsenal-card">Kali Linux</div>
<div class="arsenal-card">ELK Stack</div>
<div class="arsenal-card">Splunk</div>
<div class="arsenal-card">Wazuh</div>
<div class="arsenal-card">pfSense</div>
<div class="arsenal-card">AWS GuardDuty</div>
<div class="arsenal-card">Microsoft Sentinel</div>
<div class="arsenal-card">Maltego</div>

</div>

</section>


<section class="terminal-section">

  <h2>Curriculum</h2>

  <div class="curriculum-container">
    <div class="curriculum-sidebar">
      <button class="curriculum-btn active" onclick="switchTab(event, 'foundations')">
        Foundations & System Security
        <span class="arrow-icon">›</span>
      </button>
      <button class="curriculum-btn" onclick="switchTab(event, 'pentesting')">
        Pentesting & Ethical Hacking
        <span class="arrow-icon">›</span>
      </button>
      <button class="curriculum-btn" onclick="switchTab(event, 'redteaming')">
        Advanced Red Teaming + AI-Driven Attacks
        <span class="arrow-icon">›</span>
      </button>
      <button class="curriculum-btn" onclick="switchTab(event, 'soc-dfir')">
        SOC, DFIR + AI/ML for Defense
        <span class="arrow-icon">›</span>
      </button>
      <button class="curriculum-btn" onclick="switchTab(event, 'cloud-devsecops')">
        Cloud Security, DevSecOps & Quantum Security
        <span class="arrow-icon">›</span>
      </button>
      <button class="curriculum-btn" onclick="switchTab(event, 'capstone')">
        Capstone Project
        <span class="arrow-icon">›</span>
      </button>
    </div>

    <div class="curriculum-content">

      <!-- FOUNDATIONS -->
      <div id="foundations" class="tab-pane active">
        <div class="content-group">
          <h3>Cybersecurity Foundations & Threat Landscape</h3>
          <ul>
            <li>Domains: Red, Blue, SOC, DFIR, GRC</li>
            <li>Kill Chain & ATT&CK</li>
            <li>Threat actors & risk concepts</li>
          </ul>
        </div>
        <div class="content-group">
          <h3>Application Security, Threat Modeling & Zero Trust</h3>
          <ul>
            <li>Secure Development Lifecycle</li>
            <li>Identity, sessions & trust boundaries</li>
            <li>STRIDE & ATT&CK threat modeling</li>
            <li>Zero Trust principles & NIST SP 800-207</li>
          </ul>
        </div>
        <div class="content-group">
          <h3>Governance, Risk & Compliance Engineering</h3>
          <ul>
            <li>Governance vs Management, AI as asset</li>
            <li>KPIs, KRIs, MTTD, MTTR, board risk translation</li>
            <li>NIST CSF, 800-53, ISO 27001, FedRAMP</li>
            <li>Control inheritance, CCM, privacy engineering</li>
          </ul>
        </div>
        <div class="content-group">
          <h3>Networking for Security</h3>
          <ul>
            <li>TCP/IP & OSI fundamentals</li>
            <li>Protocol abuse (DNS, ARP, ICMP)</li>
            <li>Wireshark lab</li>
          </ul>
        </div>
        <div class="content-group">
          <h3>Linux Security & Hardening</h3>
          <ul>
            <li>Permissions & privilege model</li>
            <li>SSH & logging</li>
            <li>Hardening lab</li>
          </ul>
        </div>
      </div>
      <!-- /FOUNDATIONS -->

      <!-- PENTESTING -->
      <div id="pentesting" class="tab-pane">
        <div class="content-group">
          <h3>Recon & OSINT</h3>
          <ul>
            <li>Active vs Passive recon</li>
            <li>WHOIS, Shodan, subdomain enumeration</li>
            <li>Metadata extraction</li>
            <li>Maltego lab</li>
          </ul>
        </div>
        <div class="content-group">
          <h3>Vulnerability Scanning</h3>
          <ul>
            <li>Nmap advanced scanning</li>
            <li>Nessus/OpenVAS</li>
            <li>CVSS interpretation</li>
          </ul>
        </div>
        <div class="content-group">
          <h3>Web Application Security (OWASP Top 10)</h3>
          <ul>
            <li>Auth & session attacks</li>
            <li>SQLi</li>
            <li>XSS</li>
            <li>IDOR, CSRF, SSRF, RCE</li>
            <li>API security testing</li>
          </ul>
        </div>
        <div class="content-group">
          <h3>Exploitation Techniques</h3>
          <ul>
            <li>Metasploit framework</li>
            <li>Shell handling</li>
            <li>Privilege escalation</li>
            <li>Post-exploitation basics</li>
          </ul>
        </div>
        <div class="content-group">
          <h3>Wireless Security</h3>
          <ul>
            <li>WPA2 cracking</li>
            <li>Rogue AP lab</li>
          </ul>
        </div>
        <div class="content-group">
          <h3>Rules of Engagement & Regulatory Context</h3>
          <ul>
            <li>Legal boundaries & authorization</li>
            <li>Cyber insurance & CERT-In reporting overview</li>
          </ul>
        </div>
      </div>
      <!-- /PENTESTING -->

      <!-- RED TEAMING -->
      <div id="redteaming" class="tab-pane">
        <div class="content-group">
          <h3>Active Directory Attacks</h3>
          <ul>
            <li>Kerberos abuse</li>
            <li>Pass-the-Hash/Ticket</li>
            <li>Mimikatz lab</li>
          </ul>
        </div>
        <div class="content-group">
          <h3>Lateral Movement & Pivoting</h3>
          <ul>
            <li>SMB/RDP pivoting</li>
            <li>SOCKS tunneling</li>
            <li>Internal enumeration</li>
          </ul>
        </div>
        <div class="content-group">
          <h3>Payload Development & Evasion</h3>
          <ul>
            <li>Payload generation</li>
            <li>AV evasion & AMSI bypass</li>
            <li>Sandbox evasion</li>
          </ul>
        </div>
        <div class="content-group">
          <h3>GenAI-Assisted Offensive Security</h3>
          <ul>
            <li>AI phishing automation</li>
            <li>AI recon automation</li>
            <li>Payload mutation</li>
          </ul>
        </div>
        <div class="content-group">
          <h3>Adversarial AI Attacks</h3>
          <ul>
            <li>Data poisoning</li>
            <li>Model evasion</li>
            <li>Prompt injection</li>
            <li>Deepfake threats</li>
          </ul>
        </div>
        <div class="content-group">
          <h3>Ethics & Governance of AI Offense</h3>
          <ul>
            <li>Responsible disclosure</li>
            <li>AI attack governance</li>
          </ul>
        </div>
      </div>
      <!-- /RED TEAMING -->

      <!-- SOC / DFIR -->
      <div id="soc-dfir" class="tab-pane">
        <div class="content-group">
          <h3>SOC Operations & CTI Integration</h3>
          <ul>
            <li>SOC workflow & escalation</li>
            <li>Alert triage & playbooks</li>
            <li>CTI-driven prioritization</li>
          </ul>
        </div>
        <div class="content-group">
          <h3>Detection Engineering & Threat Hunting</h3>
          <ul>
            <li>Log ingestion (ELK/Splunk)</li>
            <li>ATT&CK-based hunts</li>
            <li>Writing detections & Sigma rules</li>
            <li>CTI-driven hypothesis hunting</li>
            <li>Alert tuning & detection coverage</li>
          </ul>
        </div>
        <div class="content-group">
          <h3>DFIR & Ransomware Recovery Engineering</h3>
          <ul>
            <li>Disk & memory acquisition</li>
            <li>Forensic artifacts & Volatility</li>
            <li>CTI in scoping incidents</li>
            <li>Immutable backups & recovery vaults</li>
          </ul>
        </div>
        <div class="content-group">
          <h3>AI/ML for Threat Detection</h3>
          <ul>
            <li>Malware classification</li>
            <li>Network anomaly detection</li>
          </ul>
        </div>
        <div class="content-group">
          <h3>AI SOC Reliability & Explainability</h3>
          <ul>
            <li>Model drift & false positives</li>
            <li>Explainable AI</li>
          </ul>
        </div>
        <div class="content-group">
          <h3>Incident Response for AI Systems</h3>
          <ul>
            <li>AI compromise & poisoning response</li>
            <li>CTI for AI threats</li>
            <li>Intelligence sharing & governance escalation</li>
          </ul>
        </div>
      </div>
      <!-- /SOC / DFIR -->

      <!-- CLOUD / DEVSECOPS -->
      <div id="cloud-devsecops" class="tab-pane">
        <div class="content-group">
          <h3>Cloud Security</h3>
          <ul>
            <li>IAM roles & policies</li>
            <li>Storage misconfigurations</li>
            <li>WAF & GuardDuty</li>
            <li>Cloud logging</li>
          </ul>
        </div>
        <div class="content-group">
          <h3>DevSecOps & Supply Chain Security</h3>
          <ul>
            <li>CI/CD security</li>
            <li>SAST/DAST</li>
            <li>Containers & Kubernetes</li>
            <li>SBOM & supply chain attacks</li>
          </ul>
        </div>
        <div class="content-group">
          <h3>API & Microservices Security</h3>
          <ul>
            <li>API auth & OAuth2</li>
            <li>Token abuse & rate limiting</li>
            <li>gRPC/GraphQL testing</li>
          </ul>
        </div>
        <div class="content-group">
          <h3>AI Security Engineering (MLOps)</h3>
          <ul>
            <li>Dataset security</li>
            <li>Model registry risks</li>
            <li>Inference API abuse</li>
          </ul>
        </div>
        <div class="content-group">
          <h3>AI Supply Chain & Model Governance</h3>
          <ul>
            <li>Third-party models</li>
            <li>Model BOM</li>
            <li>Dataset poisoning risk</li>
          </ul>
        </div>
        <div class="content-group">
          <h3>Quantum-Ready Cybersecurity</h3>
          <ul>
            <li>Quantum threat overview</li>
            <li>PQC concepts & demo</li>
          </ul>
        </div>
        <div class="content-group">
          <h3>Third-Party & Vendor Risk Management</h3>
          <ul>
            <li>Vendor risk frameworks</li>
            <li>SSPM & continuous monitoring</li>
          </ul>
        </div>
      </div>
      <!-- /CLOUD / DEVSECOPS -->

      <!-- CAPSTONE -->
      <div id="capstone" class="tab-pane">
        <div class="content-group">
          <h3>Capstone Options</h3>
          <ul>
            <li>Build AI-powered SOC that auto-detects malware traffic</li>
            <li>Full enterprise pentest including AD exploitation and forensics</li>
            <li>Hybrid Red + Blue + Purple Team cyber range simulation</li>
            <li>Post-Quantum secure cloud architecture for an organization</li>
            <li>DevSecOps pipeline with automated SAST/DAST + threat response</li>
          </ul>
        </div>
        <div class="content-group">
          <h3>Capstone Deliverables</h3>
          <ul>
            <li>Technical execution & demo (team or individual)</li>
            <li>Detailed documentation with evidence</li>
            <li>Final report containing: Executive Summary, Attack / Defense mapping to ATT&CK, IOC list & MITRE techniques</li>
            <li>Video + GitHub portfolio submission</li>
            <li>Final presentation</li>
          </ul>
        </div>
      </div>
      <!-- /CAPSTONE -->

    </div>
  </div>

</section>

<script>
function switchTab(evt, tabId) {
  const panes = document.getElementsByClassName("tab-pane");
  for (let i = 0; i < panes.length; i++) {
    panes[i].classList.remove("active");
  }
  const buttons = document.getElementsByClassName("curriculum-btn");
  for (let i = 0; i < buttons.length; i++) {
    buttons[i].classList.remove("active");
  }
  document.getElementById(tabId).classList.add("active");
  evt.currentTarget.classList.add("active");
}
</script>


</div>



