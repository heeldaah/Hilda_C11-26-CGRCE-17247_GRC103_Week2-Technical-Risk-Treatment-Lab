# INTERNATIONAL CYBERSECURITY AND DIGITAL FORENSICS ACADEMY (ICDFA)

## Governance, Risk and Compliance (GRC)

### GRC103 – Practical Laboratories

# Lab 3b: Implementing and Validating Security Controls on a Local Linux Server

---

## Submission Details

| Field | Detail |
|---|---|
| **Course** | GRC103 – Risk Assessment and Management Techniques |
| **Module** | Module 3 – Risk Treatment Strategies |
| **Institution** | International Cybersecurity and Digital Forensics Academy (ICDFA) |
| **Assignment** | Practical Laboratories – Lab Submission |
| **Student Name** | Hilda Odein Joshua-Jack |
| **Registration Number** | C11/26/CGRCE/17247 |
| **Cohort** | GRC Engineering Cohort 11 |
| **Practical Start Date / Report Date** | 8 November 2026 |
| **Due Date** | 8 November 2026, 6:47 PM WAT |
| **Maximum Marks** | 100 |
| **Practical Pass Mark** | 50% |
| **Demonstration** | Required |

---

## 1. Assignment Overview

This README describes the technical risk treatment practical completed in an isolated VirtualBox environment. The exercise used an Ubuntu Server 24.04.5 LTS virtual machine hosting an Nginx web portal and a Kali Linux virtual machine for testing.

The practical focused on four technical risks:

1. An unnecessary legacy service exposed on TCP port 8080.
2. Exposure of an archive directory through the web environment.
3. Unauthorised network access to the approved web portal.
4. Loss of portal content.

The work followed a practical risk treatment process:

1. **Baseline assessment** – recorded the initial service, port and portal state.
2. **Avoidance** – stopped and disabled the unnecessary legacy service.
3. **Mitigation** – moved the archive outside the web root, restricted its permissions and configured host firewall rules.
4. **Control validation** – tested access from an authorised source and a temporary unauthorised source.
5. **Recovery** – backed up portal content, simulated content loss, restored the content and verified it using a SHA-256 checksum.
6. **Governance** – assessed residual risks and documented proposed transfer and acceptance decisions, together with illustrative cost assumptions.

> **Scope and integrity statement:** This was a controlled training exercise in an isolated virtual environment. The fictional organisation and scenario are used for learning purposes. No production systems were accessed. Proposed arrangements and simulated governance decisions are identified as such; they are not represented as real organisational approvals or purchased services.

---

## 2. Lab Environment

| Component | Actual configuration |
|---|---|
| **Ubuntu Server VM** | Ubuntu Server 24.04.5 LTS |
| **Ubuntu IP address** | `192.168.56.10/24` on `enp0s3` |
| **Kali Linux VM** | Kali Linux testing workstation |
| **Kali IP address** | `192.168.56.20/24` on `eth0` |
| **Temporary blocked-test source** | `192.168.56.21/24` |
| **Web server** | Nginx 1.24.0 |
| **Approved portal** | `/public/`, served over TCP port 80 |
| **Firewall** | UFW |
| **Other tools used** | Nmap, curl, Python 3 HTTP server, tar, `sha256sum`, systemd |
| **Virtualisation** | Oracle VirtualBox |
| **Network arrangement** | Isolated lab network; the lab services were not exposed through bridged networking |

The Kali virtual machine also had a separate NAT interface. The lab access tests described here used the isolated lab interface.

---

## 3. Repository and Submission Contents

The repository is organised to help the assessor locate the report, calculations and practical evidence.

```text
GRC103-Technical-Risk-Treatment/
├── README.md
├── GRC103_Technical_Risk_Treatment_Report.docx
├── GRC103_Risk_Register_and_Cost_Analysis.xlsx
├── evidence-index.md
└── evidence/
    ├── 01-baseline/
    ├── 02-legacy-service-retirement/
    ├── 03-archive-protection/
    ├── 04-firewall-validation/
    └── 05-backup-and-recovery/
```

The folder names above are the **recommended repository layout**. The evidence was initially collected in the Kali directory `~/grc-treatment-evidence`. The evidence ZIP contains the 12 text files listed in Section 4. The files should be placed in the corresponding folders when the evidence is transferred into the repository.

### Main deliverables

| File | Purpose |
|---|---|
| `README.md` | Overview of the practical, environment, results and evidence locations |
| [Hilda_C11-26-CGRCE-17247_GRC103_Wk3_Technical_Risk_Treatment_Report](https://github.com/heeldaah/Hilda_C11-26-CGRCE-17247_GRC103_Week2-Technical-Risk-Treatment-Lab/blob/main/Hilda_C11-26-CGRCE-17247_GRC103_Wk3_Technical_Risk_Treatment_Report.pdf) | Technical risk treatment report documenting the lab implementation, control validation, risk assessment and treatment decisions. |
| [Hilda_C11-26-CGRCE-17247_GRC103_Wk3_Risk_Register_and_Cost_Analysis](https://github.com/heeldaah/Hilda_C11-26-CGRCE-17247_GRC103_Week2-Technical-Risk-Treatment-Lab/blob/main/Hilda_C11-26-CGRCE-17247_GRC103_Wk3_Risk_Register_and_Cost_Analysis.xlsx) | Risk register, treatment strategies, residual risk ratings and cost-benefit analysis. |
| [GRC-Evidence-Transfer](https://github.com/heeldaah/Hilda_C11-26-CGRCE-17247_GRC103_Week2-Technical-Risk-Treatment-Lab/tree/main/GRC-Evidence-Transfer) | Folder containing the practical lab evidence and supporting files. |
| [Evidence.md](https://github.com/heeldaah/Hilda_C11-26-CGRCE-17247_GRC103_Week2-Technical-Risk-Treatment-Lab/blob/main/evidence-index.md) | Text evidence collected during the practical |

> **Do not upload** passwords, private keys, VM disk images, or real personal or confidential data. The evidence ZIP is currently a separate archive on the Kali VM and must be transferred to the host computer before it can be added to GitHub.

---

## 4. Evidence Inventory and Mapping

The following 12 text files were collected in `~/grc-treatment-evidence` on Kali Linux.

| # | Evidence file | What it supports |
|---:|---|---|
| 1 | `baseline-portal.txt` | Initial response from the approved portal |
| 2 | `baseline-archive.txt` | Baseline archive exposure check |
| 3 | `baseline-legacy.txt` | Initial legacy service status |
| 4 | `baseline-ports.txt` | Baseline port scan |
| 5 | `after-legacy-retirement.txt` | Validation after the legacy service was stopped and disabled |
| 6 | `after-archive-protection.txt` | Archive protection validation |
| 7 | `after-public-portal.txt` | Approved portal availability after the security changes |
| 8 | `after-directory-protection.txt` | Directory protection validation |
| 9 | `after-firewall-allowed.txt` | Successful access from the authorised Kali address |
| 10 | `after-portal-restoration.txt` | Portal availability after restoration |
| 11 | `firewall-blocked-source.txt` | Test result from the temporary unauthorised source address |
| 12 | `ubuntu-firewall-blocked-log.txt` | Ubuntu UFW log showing the blocked connection |

The evidence archive was created as:

```text
~/grc-treatment-evidence.zip
```

Its integrity was checked using `unzip -t`, which returned:

```text
No errors detected
```

### Evidence-to-practical mapping

| Practical activity | Evidence |
|---|---|
| Baseline portal and archive checks | `baseline-portal.txt`, `baseline-archive.txt` |
| Baseline legacy service and port checks | `baseline-legacy.txt`, `baseline-ports.txt` |
| Legacy service retirement | `after-legacy-retirement.txt` |
| Archive and directory protection | `after-archive-protection.txt`, `after-directory-protection.txt` |
| Approved portal availability | `after-public-portal.txt` |
| Firewall allow test | `after-firewall-allowed.txt` |
| Firewall block test and log | `firewall-blocked-source.txt`, `ubuntu-firewall-blocked-log.txt` |
| Portal restoration | `after-portal-restoration.txt` |

The backup archive and checksum were retained on the Ubuntu VM at `/srv/grc-backups/`:

- `/srv/grc-backups/public-before-test.tgz`
- `/srv/grc-backups/portal-before.sha256`

These are server-side recovery artefacts. They are not among the 12 text files in the Kali evidence ZIP.

---

## 5. Implementation and Validation Summary

### 5.1 Baseline assessment

The initial checks established the state of the portal, archive directory and legacy service before the controls were applied. The baseline evidence is held in the four files beginning with `baseline-`.

### 5.2 Avoidance – legacy service retirement

The unnecessary Python service, identified as `grc-legacy`, was stopped and disabled. A subsequent port scan confirmed that TCP port 8080 was closed.

**Validation evidence:** `after-legacy-retirement.txt`

### 5.3 Mitigation – archive protection

The archive was moved out of the web root to:

```text
/srv/grc-private/archive
```

Its permissions were restricted, and Nginx directory listing was disabled using `autoindex off`.

**Validation evidence:** `after-archive-protection.txt` and `after-directory-protection.txt`

### 5.4 Mitigation – host firewall

UFW was configured with:

- Default-deny incoming traffic.
- Allow-outgoing traffic.
- TCP port 80 allowed from the authorised Kali address `192.168.56.20`.
- Firewall logging enabled.

The authorised Kali address successfully reached the portal. A test from the temporary source address `192.168.56.21` timed out. The Ubuntu firewall log recorded a UFW block event for the attempted connection to destination port 80.

A temporary firewall rule and Python HTTP server were used to transfer the firewall log into the evidence folder. The temporary server was stopped and the temporary firewall rule was removed after the transfer.

**Validation evidence:** `after-firewall-allowed.txt`, `firewall-blocked-source.txt` and `ubuntu-firewall-blocked-log.txt`

### 5.5 Backup and recovery

A backup of the portal content was created at `/srv/grc-backups/public-before-test.tgz`, with a SHA-256 checksum recorded in `/srv/grc-backups/portal-before.sha256`.

The portal index file was deliberately removed as a controlled loss simulation. The portal then returned HTTP 403. The content was restored from the backup, checksum verification returned `OK`, Nginx remained active and the portal returned HTTP 200.

The recorded backup timestamp was 8 November 2026 at 18:23:14 UTC. Recovery verification was recorded at 18:37:24 UTC. The observed interval was **14 minutes 10 seconds**.

This is the duration of the classroom recovery exercise, not a formally agreed production Recovery Time Objective (RTO).

**Validation evidence:** `after-portal-restoration.txt`

---

## 6. Key Results

### 6.1 Baseline and post-treatment comparison

| Control area | Baseline | Post-treatment result |
|---|---|---|
| Legacy service on TCP 8080 | Service exposed / port open | Service stopped and disabled; TCP 8080 confirmed closed |
| Approved portal on TCP 80 | Available | Remained available; authorised test returned HTTP 200 |
| Archive directory | Baseline exposure identified | Moved outside web root and access restrictions applied |
| Unauthorised source access | Not restricted by the final allowlist policy | Temporary source test timed out; UFW block event recorded |
| Authorised Kali access | Baseline access available | Continued to work; HTTP 200 |
| Portal content recovery | No recovery test yet | Restored from backup; checksum returned `OK`; HTTP 200 |

The evidence demonstrates the specific tests listed above. It does not establish full Nginx/UFW log correlation or enterprise-wide monitoring.

### 6.2 Risk register – baseline and residual assessment

Risk scores use a qualitative 5 × 5 likelihood-and-impact model. The score is calculated as likelihood multiplied by impact.

| ID | Risk | Baseline score | Baseline rating | Residual score | Residual rating | Treatment |
|---|---|---:|---|---:|---|---|
| R1 | Unnecessary legacy service exposed on TCP 8080 | 4 × 4 = 16 | High | 1 × 4 = 4 | Low | Avoid |
| R2 | Archive content exposed through the web environment | 4 × 4 = 16 | High | 1 × 4 = 4 | Low | Mitigate |
| R3 | Unauthorised network access to the web service | 3 × 5 = 15 | High | 2 × 5 = 10 | High | Mitigate |
| R4 | Loss or corruption of portal content | 3 × 4 = 12 | High | 2 × 3 = 6 | Medium | Mitigate |

Risk bands used:

| Score | Rating |
|---:|---|
| 1–4 | Low |
| 5–9 | Medium |
| 10–16 | High |
| 17–25 | Critical |

These are ordinal classroom ratings, not monetary loss values or statistically calculated probabilities.

**Residual risk explanation:**

- **R1:** The unnecessary service was retired and TCP port 8080 was confirmed closed.
- **R2:** The archive was moved outside the web root and its permissions were restricted. The residual score reflects the controls tested in this lab, not a guarantee against every possible access path.
- **R3:** Source-IP filtering reduces unauthorised network access, but it is not identity-based authentication. A compromised authorised endpoint could still present a risk.
- **R4:** The portal content was restored successfully, but the backup was stored locally. Loss of the entire VM, host or storage device remains outside the protection demonstrated.

### 6.3 Illustrative financial scenario

The financial figures below are assumptions for the exercise. They are not supplier quotations, actual expenditure or measured loss data.

| Metric | Assumption / result |
|---|---:|
| Annual occurrence rate | 0.2 |
| Estimated loss per event before treatment | $5,000 |
| Baseline ALE (0.2 × $5,000) | $1,000 |
| Estimated loss per event after treatment | $1,000 |
| Residual ALE (0.2 × $1,000) | $200 |
| Illustrative annual reduction in expected loss | $800 |
| Managed backup first-year cost ($120 setup + 12 × $20) | $360 |
| Illustrative first-year net benefit | $440 |

The calculation is scenario-based. The practical recovery test does not establish the assumed annual occurrence rate or dollar losses. Ordinal risk scores are not converted into financial savings.

### 6.4 Illustrative treatment costs

| Option | Description | Calculation | First-year estimate |
|---|---|---|---:|
| A | Hardening work | 4 hours × $25 + $50 | $150 |
| B | Managed backup | $120 setup + 12 × $20 | $360 |
| C | TLS implementation | 3 hours × $25 + $25 | $100 |
|  | **Combined illustrative estimate** | **A + B + C** | **$610** |

These options are planning assumptions only. The managed backup and TLS options were not implemented as part of this practical.

---

## 7. Risk Treatment Governance

The exercise considered the four common risk treatment responses.

| Treatment response | Application in the exercise |
|---|---|
| **Avoid** | Retired the unnecessary legacy service on TCP port 8080. |
| **Mitigate** | Restricted archive access, applied UFW source-based rules, and tested backup and restoration. |
| **Transfer** | Proposed a managed backup arrangement with defined retention, encryption, recovery testing, service levels and contractual responsibilities. No provider was engaged. |
| **Accept** | Treated the temporary limitations of HTTP and local-only backup as accepted for the isolated training exercise only. This was a simulated classroom decision, not formal approval by a production risk owner. |

Any acceptance of these limitations should be time-bound. The proposed review period is 14 days, or earlier if the system is connected to a wider network, real data is introduced, or the service is used for a business purpose.

---

## 8. Monitoring and Operational Handover – Proposed

The following checks are recommendations for a future operational environment. They were not implemented as an automated monitoring service during the lab.

| Check | Suggested frequency | Review trigger |
|---|---|---|
| Approved portal availability | Daily | Any unexpected non-200 response or outage |
| Unexpected open ports | Weekly | Any port outside the approved service list |
| Firewall rules and denied connections | Weekly | Unapproved rule changes or repeated blocked attempts |
| Archive location and permissions | Weekly | Archive moved into the web root or permissions changed |
| Backup completion and recovery test | Quarterly | Backup failure, checksum mismatch or restore failure |
| Retired legacy service status | Weekly | `grc-legacy` becomes active or enabled |

For production use, each check should have a named owner, an escalation route, evidence-retention requirements and an agreed review schedule.

---

## 9. Limitations and Assumptions

The following limitations should be considered when interpreting the results:

1. The exercise used one Ubuntu server and one Kali testing workstation in an isolated virtual environment.
2. The firewall control used source-IP restrictions. It did not provide identity-based authentication.
3. The portal used HTTP rather than HTTPS/TLS. Encryption of web traffic was not demonstrated.
4. The backup was stored locally on the Ubuntu VM. It does not protect against loss of the VM, host, storage device or site.
5. The managed backup and TLS options were proposed for analysis only. They were not purchased or implemented.
6. The exercise did not include a SIEM, production identity provider, formal vulnerability-management platform or enterprise monitoring service.
7. The lab did not complete full Nginx/UFW log correlation. The UFW block event was captured and retained as evidence.
8. Risk scores and financial estimates are illustrative assessment inputs, not production measurements.
9. Risk acceptance and provider arrangements were simulated for the coursework and were not approved by a real business owner.

---

## 10. Security and Integrity Declaration

- The practical was conducted in an isolated VirtualBox lab environment.
- No production systems were accessed or changed.
- The lab used dummy training content rather than real customer or business records.
- Passwords, private keys and VM disk images are excluded from the submission.
- Transfer arrangements, provider commitments and risk acceptance decisions are presented as proposals or simulations where applicable.
- Risk ratings are qualitative classroom scores.
- Financial figures are assumptions and are not presented as actual costs or guaranteed savings.
- Practical results are reported according to the evidence collected.

---

## 11. Marking Criteria Alignment

The following table reproduces the assessment areas and mark allocation from the assignment brief and maps them to the relevant submission materials.

| Assessment criterion | Marks | Where addressed |
|---|---:|---|
| Baseline and risk register | 15 | Sections 4 and 6; baseline evidence; Excel workbook |
| Avoidance and continuity tests | 10 | Sections 5 and 6; legacy service retirement and portal availability evidence |
| Archive hardening and firewall validation | 25 | Sections 5 and 6; archive and firewall evidence |
| Log correlation | 10 | UFW block event and firewall log evidence; limitations are disclosed in Section 9 |
| Backup and verified restoration | 15 | Section 5.5; recovery evidence and server-side backup/checksum artefacts |
| Treatment governance and financial reasoning | 15 | Sections 6 and 7; Excel workbook |
| Reproducible evidence and management report | 10 | This README, Word report, evidence inventory and evidence files |
| **Total** | **100** | |

---

## 12. References

- Ubuntu Server documentation – Firewall: https://ubuntu.com/server/docs/how-to/security/firewalls/
- NGINX documentation – `ngx_http_autoindex_module`: https://nginx.org/en/docs/http/ngx_http_autoindex_module.html
- NGINX documentation – `ngx_http_log_module`: https://nginx.org/en/docs/http/ngx_http_log_module.html
- ICDFA GRC103 Module 3 – Risk Treatment Strategies course materials.

---

**End of README**

*Student Name: Hilda Odein Joshua-Jack | Registration Number: C11/26/CGRCE/17247*
