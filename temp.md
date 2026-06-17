
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
          <h3>Advanced Red Teaming + AI-Driven Attacks</h3>
          <ul>
            <li>Adversary Emulation & Evasion Strategies</li>
            <li>Automating Payloads with AI/ML Models</li>
            <li>C2 Infrastructure Deployment</li>
          </ul>
        </div>
      </div>
      <!-- /RED TEAMING -->

      <!-- SOC / DFIR -->
      <div id="soc-dfir" class="tab-pane">
        <div class="content-group">
          <h3>SOC, DFIR + AI/ML for Defense</h3>
          <ul>
            <li>Log Analysis & SIEM Deployment Engines</li>
            <li>Digital Forensics & Incident Response Workflows</li>
            <li>Anomalous Traffic Detection via TinyML Models</li>
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
          </ul>
        </div>
      </div>
      <!-- /CLOUD / DEVSECOPS -->

      <!-- CAPSTONE -->
      <div id="capstone" class="tab-pane">
        <div class="content-group">
          <h3>Capstone Project</h3>
          <ul>
            <li>Enterprise-Scale Architecture Simulation</li>
            <li>Security Integrity Validation Pipeline Auditing</li>
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