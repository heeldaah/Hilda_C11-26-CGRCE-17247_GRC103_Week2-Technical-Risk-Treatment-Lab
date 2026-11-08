# README — GRC103 Technical Risk Treatment Lab

## Implementing and Validating Security Controls on a Local Linux Server

---

| Field | Detail |
|---|---|
| **Course** | GRC103 — Risk Assessment and Management Techniques |
| **Module** | Module 3: Risk Treatment Strategies |
| **Institution** | International Cybersecurity and Digital Forensics Academy (ICDFA) |
| **Assignment** | Practical Laboratories — Lab Submission |
| **Student Name** | Hilda Odein Joshua-Jack |
| **Registration Number** | C11/26/CGRCE/17247 |
| **Submission Date** | 08 Nov 2026 |
| **Due Date** | 08 Nov 2026, 6:47 PM WAT |
| **Maximum Marks** | 100 |
| **Practical Pass Mark** | 50% |
| **Demonstration** | Required |

---

## 1. Assignment Overview

This README documents the technical risk treatment laboratory completed for **Northbridge Services**, a fictional organisation whose internal document server exposes an unnecessary legacy service and a directory of dummy records. The lab implements and validates preventive, detective and corrective controls on an isolated Ubuntu Server 24.04 LTS / Kali Linux environment, and uses the measured results to justify **avoidance, mitigation, transfer and acceptance** decisions.

The work follows the full risk treatment lifecycle:

1. **Baseline assessment** — measure exposed services and score risks.
2. **Avoidance** — retire the unnecessary legacy endpoint.
3. **Mitigation** — harden the archive directory and apply source-based firewall rules.
4. **Detection** — correlate Nginx and UFW logs with controlled test traffic.
5. **Recovery** — back up, delete, restore and hash-verify the approved portal.
6. **Governance** — evaluate transfer/acceptance options, compare costs and reassess residual risk.

> **Important:** All data, files, approvals and financial figures in this lab are fictional or simulated. Approvals are labelled *simulated* throughout. No production systems were touched.

---

## 2. Lab Environment

| Component | Specification |
|---|---|
| **Ubuntu Server VM** | Ubuntu Server 24.04 LTS — 2 vCPUs, 2–4 GB RAM, 25 GB disk |
| **Kali Linux VM** | Kali Linux — 2 vCPUs, 2–4 GB RAM, 25 GB disk |
| **Network** | Isolated internal / host-only network (no bridged or public exposure) |
| **Key software** | Nginx, UFW, Python 3 (`http.server`), Nmap, curl, tar, sha256sum, systemd |
| **Ubuntu IP (isolated)** | *[e.g. 192.168.56.20 — replace with actual]* |
| **Kali IP (isolated)** | *[e.g. 192.168.56.10 — replace with actual]* |
| **Blocked-test source IP** | *[e.g. 192.168.56.11 — second address on Kali, removed after test]* |

A clean VM checkpoint was taken **before** any configuration. A temporary NAT adapter was used only for package installation and disconnected before the intentionally exposed lab services were created.

---

## 3. Repository / Submission Contents

GRC103-Lab-Submission/
├── README.md ← this file
├── GRC103_Lab_Report.pdf ← full written report
├── evidence/
│ ├── 01-baseline/
│ ├── 02-avoidance/
│ ├── 03-preventive-controls/
│ ├── 04-logs-correlation/
│ ├── 05-backup-recovery/
│ └── 06-governance/
├── configs/
│ ├── grc-lab.nginx.conf
│ ├── grc-legacy.service
│ └── ufw-status.txt
├── logs/
│ ├── nginx-grc_access-extract.log ← redacted
│ ├── ufw-block-extract.log ← redacted
│ └── journal-ufw-extract.txt ← redacted
├── calculations/
│ └── treatment-cost-workbook.xlsx
└── evidence-index.md


> **Excluded from submission (per brief):** passwords, private keys, VM disk images, any real or personal data.

---

## 4. Environment Build Summary

| Step | Action | Evidence |
|---|---|---|
| 1 | Installed `nginx ufw python3 curl` on Ubuntu; recorded addresses with `ip -br addr` | `evidence/01-baseline/ip-addr.txt` |
| 2 | Created fictional content (`/var/www/grc-lab/public`, `/var/www/grc-lab/archive`, `/srv/grc-legacy`) and the `grc-legacy.service` systemd unit on port 8080 | `configs/grc-legacy.service` |
| 3 | Configured the `grc-lab` Nginx site with combined access logging and `autoindex on` (baseline state) | `configs/grc-lab.nginx.conf` |
| 4 | Collected baseline from Kali (Nmap, curl, UFW status, `ss -lntp`) | `evidence/01-baseline/` |
| 5 | Built the baseline risk register (R1–R4) | `evidence/01-baseline/risk-register.*` |
| 6 | Applied **avoidance**: disabled and stopped `grc-legacy`; rollback tested, then reapplied | `evidence/02-avoidance/` |
| 7 | Applied **mitigation**: moved archive to `/srv/grc-private`, `chmod go-rwx`, Nginx `deny all` + `autoindex off` | `evidence/03-preventive-controls/` |
| 8 | Applied **UFW**: default deny incoming / allow outgoing, allow port 80 from Kali only, logging on | `configs/ufw-status.txt` |
| 9 | Correlated permitted and blocked requests with Nginx and UFW logs | `evidence/04-logs-correlation/` |
| 10 | Backed up portal, deleted `index.html`, restored via `tar`, verified with `sha256sum -c` | `evidence/05-backup-recovery/` |
| 11 | Modelled **transfer** (managed backup offer) and **acceptance** (plain HTTP, 14-day expiry, simulated approval) | `evidence/06-governance/` |
| 12 | Cost comparison (Options A/B/C), illustrative ALE calculation, residual risk reassessment | `calculations/treatment-cost-workbook.xlsx` |

---

## 5. Evidence Index (Mapping Evidence → Practical Steps)

### 5.1 Required Evidence Deliverables

| # | Required Evidence (from brief) | Location | Status |
|---|---|---|---|
| 1 | Baseline assessment & risk register (scans, screenshots, ratings) | `evidence/01-baseline/` | ✅ |
| 2 | Avoidance & preventive controls (legacy retirement, archive protection, firewall config) | `evidence/02-avoidance/`, `evidence/03-preventive-controls/`, `configs/` | ✅ |
| 3 | Control validation & logs (permitted/blocked tests, timestamps, source IPs, log extracts) | `evidence/04-logs-correlation/`, `logs/` | ✅ |
| 4 | Backup & recovery (backup, simulated loss, restore, hash verification, recovery time) | `evidence/05-backup-recovery/` | ✅ |
| 5 | Treatment decisions & financial analysis (four strategies, costs, residual risk) | `evidence/06-governance/`, `calculations/` | ✅ |
| 6 | Management report & evidence index (exec summary, monitoring plan, change/rollback records, README) | `GRC103_Lab_Report.pdf`, this README, `evidence-index.md` | ✅ |

### 5.2 Step-by-Step Evidence Map

| Step | Description | Evidence File(s) | Test / Validation |
|---|---|---|---|
| 1 | Ubuntu preparation & IP recording | `ip-addr.txt` | Isolated adapter active |
| 2 | Fictional content + legacy service created | `grc-legacy.service`, `legacy-service-active.txt` | `systemctl status grc-legacy` |
| 3 | Nginx lab site configured | `grc-lab.nginx.conf`, `nginx-t.txt` | `nginx -t` passed |
| 4 | Baseline collection (Kali) | `baseline-time.txt`, `baseline-ports.txt`, `baseline-web.txt`, `baseline-listing.txt`, `baseline-file.txt`, `baseline-legacy.txt` | Ports 80 & 8080 reachable; archive listable; dummy file retrievable |
| 5 | Baseline risk register | `risk-register.xlsx` / `.md` | R1–R4 scored (L×I, 1–25 bands) |
| 6 | Avoidance — legacy retired | `avoidance-ports.txt`, `avoidance-legacy.txt`, `rollback-record.txt` | 8080 closed; port 80 still 200; rollback tested |
| 7 | Archive removed from web root | `archive-403-public-200.txt`, `nginx-archive-config.txt` | `/public/` = 200; `/archive/` = 403; `/archive/dummy-records.txt` = 403; `/` = 403 |
| 8 | UFW source-based restriction | `ufw-status.txt`, `ufw-rules-numbered.txt`, `allowed-kali-200.txt`, `blocked-source-timeout.txt` | Kali = 200; unapproved source = timeout |
| 9 | Log correlation | `nginx-grc_access-extract.log`, `ufw-block-extract.log`, `journal-ufw-extract.txt`, `evidence-table.xlsx` | Timestamped correlation; blocked traffic absent from Nginx |
| 10 | Backup & recovery | `public-before-test.tgz`, `portal-before.sha256`, `recovery-timestamps.txt`, `hash-verify-ok.txt`, `post-restore-200.txt` | Delete → 404, restore → `sha256sum -c` OK, HTTP 200, recovery time calculated |
| 11 | Transfer & acceptance | `transfer-analysis.md`, `acceptance-record.md` (both marked simulated) | Provider duties, clause draft, 14-day expiry, review triggers |
| 12 | Cost & residual risk | `treatment-cost-workbook.xlsx`, `residual-risk-register.xlsx` | Option comparison, ALE (illustrative), R1–R4 reassessed |

---

## 6. Key Results Summary

### 6.1 Baseline vs. Post-Treatment

| Observation | Baseline | Post-Treatment |
|---|---|---|
| Port 8080 (legacy) | Open | **Closed** (service disabled) |
| Port 80 (approved portal) | Open, 200 | Open, 200 — availability preserved |
| `/archive/` listing | HTTP 200, files listed | **HTTP 403** |
| `/archive/dummy-records.txt` | HTTP 200, retrievable | **HTTP 403** |
| `/` root listing | Directory listing visible | **HTTP 403** (autoindex off) |
| Access from unapproved source | Permitted | **Timed out / blocked (UFW)** |
| Access from approved Kali IP | Permitted | Permitted (200) |
| Nginx/UFW log correlation | N/A | Blocked traffic never reaches Nginx; UFW `BLOCK` logged |

### 6.2 Risk Register — Baseline and Residual

| ID | Risk | Asset | Baseline Score (L×I) | Band | Residual Score (L×I) | Band | Treatment |
|---|---|---|---|---|---|---|---|
| R1 | Unnecessary legacy service | Legacy endpoint (8080) | *[e.g. 4×4=16]* | High | *[1×4=4]* | Low | Avoidance |
| R2 | Exposed archive directory | Dummy records | *[4×4=16]* | High | *[2×3=6]* | Medium | Mitigation |
| R3 | Unauthorised network access | Portal (80) | *[3×4=12]* | High | *[2×3=6]* | Medium | Mitigation (UFW) |
| R4 | Loss of portal content | `public/index.html` | *[2×4=8]* | Medium | *[2×2=4]* | Low | Corrective (backup) |

> Scores are **ordinal classroom thresholds**, not monetary loss or statistical probabilities. *[Replace example values with your measured results.]* Untested outcomes are marked as such rather than assumed effective.

### 6.3 Illustrative Financial Scenario (assumptions only)

| Metric | Value |
|---|---|
| Annual occurrence rate (assumed) | 0.2 |
| Loss per event (assumed) | $5,000 |
| Baseline ALE (0.2 × $5,000) | **$1,000** |
| Reduced loss per event with tested recovery (assumed) | $1,000 |
| Residual ALE (0.2 × $1,000) | **$200** |
| Annual benefit | **$800** |
| Option B first-year cost ($120 setup + 12 × $20) | **$360** |
| First-year net benefit (Option B) | **$440** |

> All figures are **assumptions**. The recovery test does not establish these estimates, and ordinal risk scores are **never** converted into dollar savings.

### 6.4 Treatment Options (Fictional, First-Year)

| Option | Description | Calculation | First-Year Cost |
|---|---|---|---|
| A | Hardening work | 4 h × $25 + $50 | **$150** |
| B | Managed backup | $120 + 12 × $20 | **$360** |
| C | TLS deployment | 3 h × $25 + $25 | **$100** |

**Selected combination:** *[A + B — justify with measured results]*. Excluded costs documented in the workbook.

---

## 7. Monitoring & Operational Handover (Proposed)

| Check | Owner | Frequency | Threshold | Escalation |
|---|---|---|---|---|
| Approved portal availability | *[Ops/GRC analyst]* | Daily | Any non-200 or >5 min outage | Service owner |
| Unexpected open ports | *[GRC analyst]* | Weekly | Any port other than 80 | Security lead |
| Recovery-test success | *[Ops]* | Quarterly | Hash mismatch or restore failure | Backup owner |
| Configuration drift (retired legacy service) | *[Ops]* | Weekly | `grc-legacy` active/enabled | Change board |

> Lab results are **measured**; ongoing monitoring above is **proposed**.

---

## 8. Change & Rollback Records

| Change | Approval | Rollback | Status |
|---|---|---|---|
| Retire `grc-legacy` (disable/stop) | *Simulated — student* | `sudo systemctl enable --now grc-legacy` | Rollback tested, treatment reapplied |
| Move archive → `/srv/grc-private` | *Simulated — student* | `mv` back + Nginx reload | Applied |
| UFW deny/allow rules | *Simulated — student* | `sudo ufw disable` (console) then reapply | Applied |
| Portal backup & restore test | *Simulated — student* | `tar -xzpf ... -C /var/www/grc-lab` | Verified |

---

## 9. Reproducibility Notes

1. Take a clean VM checkpoint before building.
2. Keep the Ubuntu VM **console** open throughout (UFW can lock out SSH-only sessions).
3. Replace all sample IPs with your **actual isolated-network addresses**.
4. Remove any broad UFW allow rules on the disposable lab VM before firewall tests.
5. Disconnect the temporary NAT adapter before creating exposed lab services.
6. Do not fabricate missing log events — document collection problems and distinguish an **unverified** control from a **failed** control.

---

## 10. Security & Integrity Declaration

- Only **fictional/dummy** files were used; no real or personal data.
- No bridged networking or public exposure occurred.
- Secrets, passwords, private keys and VM disk images are **not** included in this submission.
- All approvals, provider offers, contracts and insurance references are **simulated** for the exercise.
- Ordinal risk scores are not interpreted as monetary loss or statistical probabilities.
- Financial figures are explicitly labelled **assumptions** and do not claim real-world validity.

---

## 11. Marking Criteria Alignment

| Criterion | Marks | Where Addressed |
|---|---|---|
| Baseline and risk register | 15 | §5.2 Steps 4–5, `evidence/01-baseline/` |
| Avoidance and continuity tests | 10 | §6.1, `evidence/02-avoidance/` |
| Archive hardening and firewall validation | 25 | §6.1, `evidence/03-preventive-controls/`, `configs/` |
| Log correlation | 10 | §5.2 Step 9, `evidence/04-logs-correlation/`, `logs/` |
| Backup and verified restoration | 15 | §6.1, `evidence/05-backup-recovery/` |
| Treatment governance and financial reasoning | 15 | §6.3–6.4, `evidence/06-governance/`, `calculations/` |
| Reproducible evidence and management report | 10 | This README, `GRC103_Lab_Report.pdf`, `evidence-index.md` |
| **Total** | **100** | |

---

## 12. References

- Ubuntu Server firewall documentation — https://ubuntu.com/server/docs/firewalls/
- NGINX directory listing (`ngx_http_autoindex_module`) — https://nginx.org/en/docs/http/ngx_http_autoindex_module.html
- NGINX access logging (`ngx_http_log_module`) — https://nginx.org/en/docs/http/ngx_http_log_module.html
- Course basis: ICDFA Risk Treatment Strategies slides, GRC103 Module 3

---

**End of README**
*Student Name: Hilda Odein Joshua-Jack | Registration Number: C11/26/CGRCE/17247*
