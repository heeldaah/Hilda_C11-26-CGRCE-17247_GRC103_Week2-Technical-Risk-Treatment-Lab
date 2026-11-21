
# GRC103 Technical Risk Treatment – Evidence Index

**Candidate:** Hilda Odein Joshua-Jack  
**Registration Number:** C11/26/CGRCE/17247  
**Lab Start Date:** 8 November 2026

## 1. Purpose

This document provides an index of the practical evidence collected during the GRC103 Technical Risk Treatment laboratory. The evidence documents the initial security assessment, implementation of security controls, validation tests and recovery activities performed in the isolated virtual lab environment.

## 2. Evidence Files

| Evidence File | Description |
|---|---|
| `baseline-portal.txt` | Records the initial state and accessibility of the public web portal. |
| `baseline-archive.txt` | Documents the initial archive exposure assessment. |
| `baseline-legacy.txt` | Records the initial status of the legacy service. |
| `baseline-ports.txt` | Captures the baseline network port assessment. |
| `after-legacy-retirement.txt` | Provides evidence that the legacy service was retired. |
| `after-archive-protection.txt` | Documents the archive protection changes. |
| `after-public-portal.txt` | Records the public portal status following control implementation. |
| `after-directory-protection.txt` | Captures the directory protection configuration and validation. |
| `after-firewall-allowed.txt` | Records successful access from the authorised Kali test machine. |
| `firewall-blocked-source.txt` | Documents the unsuccessful access attempt from an unauthorised test source. |
| `ubuntu-firewall-blocked-log.txt` | Contains the Ubuntu UFW log evidence of the blocked connection attempt. |
| `after-portal-restoration.txt` | Records the portal restoration and recovery validation. |

## 3. Evidence Storage

The evidence files are packaged in:

`grc-treatment-evidence.zip`

The archive is included in this GitHub repository for review and verification.

## 4. Validation Summary

The collected evidence supports the following laboratory outcomes:

- Retirement of the unnecessary legacy service on TCP port 8080.
- Protection of the archive directory from public access.
- Restriction of HTTP access to the authorised Kali test source.
- Verification of successful authorised access and blocked unauthorised access.
- Restoration of the public portal from a local backup.
- Successful checksum verification following restoration.

## 5. Evidence Limitations

The evidence was collected in an isolated classroom virtual environment. It demonstrates the controls tested during the exercise and should not be interpreted as proof of production deployment or enterprise-wide security effectiveness.
