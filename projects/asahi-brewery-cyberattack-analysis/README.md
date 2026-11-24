# Asahi Brewery Cyberattack — Case Study & Risk Assessment

**Academic team project | February–March 2026**  
**Role:** Team Lead & Primary Contributor (team project)

## Demo and project material
- **Presentation / demo material:** [Open Google Drive folder](https://drive.google.com/drive/u/1/folders/1dyLd0hAhUpmWoQpbUoqca6jt1W8kzZDV)
- This restores the original shared Drive folder from the project CV. It is a folder link, not a direct video URL.

## Overview
This project studies the 2025 ransomware incident affecting Asahi Group Holdings. It organizes information from public reporting and threat-intelligence references into an attack-chain analysis, maps relevant techniques to MITRE ATT&CK, evaluates impact using the CIA Triad, builds a qualitative risk assessment, and proposes risk-reduction measures aligned with NIST Cybersecurity Framework concepts.

**Important scope clarification:** This is primarily a research-based case study. The submitted report states that the analysis relied mainly on public sources, some details involved assumptions because internal evidence was unavailable, and a full real-world attack simulation was not performed. This repository does not claim that the project reproduced Asahi's production environment or the complete incident.

## Incident snapshot presented in the project
The presentation describes the incident as follows:
- Detection time cited: **07:00 on 29 September 2025**.
- Threat actor cited: **Qilin**, described as a Ransomware-as-a-Service group.
- Attack pattern: ransomware combined with data theft (**double extortion**).
- Data volume cited: approximately **27 GB** of internal data.
- Business disruption discussed: manufacturing, order processing, customer contact, and logistics operations were affected.

The presentation contains two different customer/employee record figures: approximately **1.9 million** in the incident overview and **115,513** in the CIA-impact slide. This README intentionally does not collapse those figures into one definitive number; verify the underlying public source before quoting a precise count.

## Analysis workflow

\`\`\`mermaid
flowchart TD
    A[Collect public incident information] --> B[Build attack-chain timeline]
    B --> C[Map techniques to MITRE ATT&CK]
    C --> D[Assess impact with CIA Triad]
    D --> E[Prioritize risks with a risk matrix]
    E --> F[Map gaps to NIST CSF]
    F --> G[Recommend defense-in-depth controls]
\`\`\`

## Attack-chain analysis
The presentation groups the incident narrative into five stages:
1. **Reconnaissance:** public OSINT and target profiling, including references to LinkedIn and IT / logistics personnel.
2. **Initial access:** spear-phishing using a fake logistics invoice / partner identity.
3. **Execution:** a macro-enabled document is described as triggering a PowerShell script and retrieving malware from command-and-control infrastructure.
4. **Lateral movement:** PsExec and RDP over SMB are discussed as methods used to move through the network and reach business systems; the presentation cites a dwell time of approximately 10 days.
5. **Impact:** ransomware deployment, data theft, deletion of Shadow Copies / recovery data, and disruption to manufacturing and logistics.

## MITRE ATT&CK mapping
The report discusses techniques including:
- Initial access: \`T1566.001\`, \`T1566.002\`, \`T1078.002\`.
- Execution: \`T1204.002\`, \`T1059.001\`.
- Persistence / credential access: \`T1547.001\`, \`T1053.005\`, \`T1003.001\`.
- Lateral movement: \`T1021.001\`, \`T1021.002\`.
- Defense evasion / recovery inhibition: \`T1562.001\`, \`T1490\`.
- Exfiltration / impact: \`T1041\`, \`T1486\`.

These are analytical mappings based on sources referenced in the report. They should not be interpreted as independent forensic evidence collected by the student team for every technique.

## Impact assessment — CIA Triad

| Security property | Assessment in presentation | Main impact discussed |
|---|---|---|
| Confidentiality | High | Data theft and exposure of internal / customer / employee information |
| Integrity | Medium | Backup deletion and loss of confidence in system logs / data integrity |
| Availability | Very severe | Disruption of factories, order management, and logistics operations |

## Risk matrix and NIST CSF observations
The presentation highlights three major risks in its 5×5 matrix:
- Ransomware deployment and encryption: critical impact.
- Phishing / credential theft: high risk because it can enable the wider intrusion.
- Data exfiltration: high risk due to regulatory and reputational consequences.

The NIST CSF review in the presentation identifies these weaknesses:

| Function | Observation in the case study |
|---|---|
| Identify | Asset inventory is discussed, but remote-access risk assessment is described as insufficient |
| Protect | Weak MFA and a flat network without sufficient segmentation |
| Detect | The presentation cites a 10-day dwell time without effective behavioral alerting |
| Respond / Recover | Recovery and logistics stabilization are described as slow |

## Proposed Defense-in-Depth architecture
The project proposes five layers of controls. These are **recommendations**, not controls deployed by the student team inside Asahi:
1. **Endpoint:** EDR / NGAV and endpoint protection.
2. **Network:** firewall, IDS/IPS, and network segmentation.
3. **Identity:** strong MFA, PAM, least privilege, and Zero Trust.
4. **Monitoring:** centralized SIEM / SOAR, threat intelligence, and SOC monitoring.
5. **Recovery:** immutable / offline backup and disaster recovery planning.

The presentation also recommends phishing-awareness training, an incident-response playbook, and regular recovery exercises.

## Financial and operational impact figures cited in the presentation
The presentation gives an estimated total impact of **10–15 billion JPY** and separately illustrates approximately 5 billion JPY in lost revenue, approximately 2 billion JPY in reduced profit, and a 4% decrease in domestic beer production. These should be treated as figures quoted by the class presentation and checked against its underlying references before external publication.

## Tools and frameworks
- VMware, Kali Linux, Windows 10, and Metasploit are listed in the project toolset / report.
- MITRE ATT&CK.
- NIST Cybersecurity Framework (CSF 2.0).
- CIA Triad and a 5×5 risk matrix.

## Team and personal contribution
**Team project.** The submitted report lists Tiêu Hoàng Phúc, Nguyễn Nam Trường, and Nguyễn Phú Đạt.

**My role — Team Lead & Primary Contributor:** Led the project work and handled the case-study research and analysis deliverables, including the attack-chain breakdown, MITRE ATT&CK mapping, CIA Triad assessment, risk matrix, NIST CSF mapping, and Defense-in-Depth recommendations. The other team members are also credited in the academic report.

## Limitations
- The analysis relies primarily on public sources rather than Asahi's internal forensic data.
- Some details are analytical assumptions because complete internal evidence is unavailable.
- The submitted report explicitly states that a full attack simulation was not performed.
- Findings and technique mappings should be interpreted within the context and scope of the academic case study.

## References cited in the academic report
- [MITRE ATT&CK](https://attack.mitre.org/)
- [NIST Cybersecurity Framework 2.0](https://www.nist.gov/cyberframework)
- [Qualys — Qilin Ransomware Threat Analysis](https://blog.qualys.com/vulnerabilities-threat-research/2025/10/qilin-ransomware)
- [The Japan Times — Asahi Beer Shortages After Cyberattack](https://www.japantimes.co.jp/business/2025/10/10/asahi-beer-shortage-ransomware/)
- [Lockheed Martin — Cyber Kill Chain](https://www.lockheedmartin.com/en-us/capabilities/cyber/cyber-kill-chain.html)

## Safety and responsible use
This repository is for academic security analysis and defensive learning. Do not use attack-chain descriptions to target systems. Any hands-on testing must be performed only in an isolated lab and on systems for which you have explicit authorization.
