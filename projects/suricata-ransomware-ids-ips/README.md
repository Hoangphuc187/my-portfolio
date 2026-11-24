# Ransomware Detection & Prevention with Suricata (IDS/IPS)

**Academic lab project | September–November 2025**  
**Role:** Team Lead & Primary Implementer (team project)

## Demo and project material
- **Demo / project folder:** [Open Google Drive folder](https://drive.google.com/drive/u/1/folders/1ebBT8lkQBgiXf-wst0CD1hmta1t3FRg9)
- This is the original shared Drive folder from the project CV. It is a folder link, not a direct video URL.

## Overview
This project builds an isolated three-VM network lab to detect and block selected ransomware-like network behaviors with Suricata. Suricata is deployed in **IPS Inline mode using AF-PACKET**, allowing traffic to be inspected and matching traffic to be dropped before it reaches the Windows test endpoint.

The project focuses on observable network indicators associated with the simulated ransomware scenarios: suspicious HTTP downloads of executable/install packages, repeated SMB file-write activity, and unusual DNS queries that may resemble beaconing. The presentation states that WannaCry traffic was the main study subject and that PCAP analysis was performed in offline and inline contexts.

## Lab topology

\`\`\`mermaid
flowchart LR
    A[Kali Linux\\nAttacker / traffic generator] -->|ens33 side| B[Ubuntu Linux\\nSuricata IPS Inline]
    B -->|ens37 side| C[Windows 10\\nVictim / test endpoint]
\`\`\`

- **Kali Linux:** generates controlled test traffic inside the isolated lab.
- **Ubuntu Linux:** runs Suricata and inspects traffic across two interfaces (\`ens33\` and \`ens37\`) in AF-PACKET IPS mode.
- **Windows 10:** receives forwarded traffic and acts as the test endpoint.

> Interface names and IP addresses are specific to the original VM setup. Adapt them before reusing any configuration.

## Technology stack
- Suricata IDS/IPS
- Ubuntu Linux, Kali Linux, Windows 10
- VMware Workstation
- AF-PACKET (IPS Inline)
- Suricata custom rules, PCAP analysis, Windows Event Viewer

## Configuration highlights
The implementation report describes:
- \`host-os-policy\` for traffic normalization / host-specific policy handling.
- Two AF-PACKET interfaces in IPS mode: \`ens33\` and \`ens37\`.
- Protocol port groups: HTTP \`80, 8080\`; DNS \`53\`; SMB \`445, 139\`; TLS \`443\`.
- Rule files: \`ips_demo.rules\` and \`ransomware.rules\`.

An illustrative configuration fragment is in [\`configs/af-packet-inline.example.yaml\`](configs/af-packet-inline.example.yaml). It is an example fragment, not a complete drop-in \`suricata.yaml\`.

## Detection scenarios and custom rules

| Scenario | Detection focus | Rule behavior |
|---|---|---|
| Suspicious HTTP download | \`.exe\` / \`.msi\` indicators in HTTP URI or response traffic | Alert on suspicious download pattern |
| SMB write burst | Repeated SMB \`Write AndX\` activity within a short period | Alert on behavior that may indicate mass file modification |
| DNS beacon-like query | Long, random-looking DNS labels | Alert for investigation |
| SMB prevention test | SMB write behavior matching the lab rule | Drop matching traffic in IPS Inline mode |

The example rules use \`content\`, \`pcre\`, \`flow\`, \`threshold\`, \`sid\`, and the \`alert\` / \`drop\` actions. See [\`rules/ransomware.rules.example\`](rules/ransomware.rules.example).

## Monitoring and validation
The team tested the rules with simulated traffic and PCAP data. The report describes analysis of Suricata output (\`fast.log\` and \`eve.json\`) and correlation with Windows Event Viewer records, including event IDs \`4688\`, \`4663\`, and \`7036\`, to reconstruct an incident timeline.

The presentation reports detection of the main attack behaviors in its test scenarios. It also notes that some redundant DNS alerts appeared and that rule tuning is needed. **The reported result is limited to the tested lab scenarios; it is not a claim of 100% detection against all ransomware or production networks.**

## Risk observations from the project

| ID | Weakness identified in the lab review | Risk stated in presentation | Suggested mitigation |
|---|---|---|---|
| V1 | Unpatched SMBv1 / MS17-010 exposure | High | Patch affected systems, disable SMBv1, restrict SMB between network segments |
| V2 | Uncontrolled outbound DNS | Medium | DNS filtering, restrict direct external DNS, consider sinkhole controls |
| V3 | Executable downloads allowed over HTTP | Medium | Proxy filtering, restrict executable downloads, use allowlists |
| V4 | Noisy / insufficiently tuned rules | Low–Medium | Tune rules and thresholds; consider SIEM integration |

## Reproducing the lab
1. Create three VMs in VMware: Kali Linux, Ubuntu Linux, and Windows 10.
2. Configure Ubuntu with two network interfaces so it can inspect traffic between the test-traffic VM and the Windows endpoint.
3. Install Suricata on Ubuntu and configure AF-PACKET IPS Inline mode for the lab interfaces.
4. Configure the relevant protocol port groups (HTTP, DNS, SMB, and TLS).
5. Add local rules after adapting destination addresses and validating syntax for your Suricata version.
6. Validate the configuration and generate only benign/safe test traffic or use sanitized PCAP files on an isolated network.
7. Review Suricata logs and Windows Event Viewer records, then document each test case and result.

This repository provides example fragments and project documentation, not VM images or a complete production configuration.

## Limitations and next steps
- Controlled academic lab; not deployed on a real enterprise network.
- The presentation states that the study focused mainly on WannaCry-related traffic and did not expand to multiple ransomware families.
- Detection coverage is limited to the HTTP, SMB, and DNS patterns tested.
- False-positive tuning and broader rule coverage are required.
- EDR / SIEM integration was identified as future work, not as an implemented feature.

## Team and personal contribution
**Team project.** The submitted project lists Tiêu Hoàng Phúc and Nguyễn Phú Đạt.

**My role — Team Lead & Primary Implementer:** Led the project and was responsible for the lab implementation workflow, including the three-VM topology, Suricata AF-PACKET IPS Inline setup, custom HTTP/SMB/DNS rule development, controlled test execution, and review of Suricata and Windows event logs. The other team member is also credited in the academic report.

## Safety notice
All tests must be performed in an isolated lab or on systems for which you have explicit authorization. Do not upload live malware, weaponized payloads, credentials, private network details, or PCAP files containing personal or organizational data.
