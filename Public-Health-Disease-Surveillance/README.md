# Disease Surveillance Architecture Development Project

## Overview
This project simulates a multi-hospital health architecture with a centralized Health Information Exchange (HIE) to explore real-world implementation of Electronic Health Records (EHR), data exchange standards (FHIR), and outbreak surveillance using synthetic data. Technologies include OpenEMR, HAPI-FHIR, Docker, Postman, and Synthea, enabling end-to-end simulation of public health informatics infrastructure.

---

## Architecture Diagram
**5 Virtual Machines (VMs)**:
- **4 Hospitals**: Aspirus, Portage Health, BCMH, MGH
- **1 HIE**: UPHIE (using HAPI-FHIR)

---

## Part 1: Virtual Machine Configuration and Network Testing

**Objective**: Set up VMs with static IPs and confirm inter-VM connectivity.

| Hospital  | OS Compatible (HAPI/OpenEMR) | IP Address      | Ping Successful |
|-----------|-------------------------------|------------------|------------------|
| Aspirus   | Yes / Yes                    | 192.168.17.114   | Yes              |
| Portage   | Yes / Yes                    | 192.168.17.115   | Yes              |
| BCMH      | Yes / Yes                    | 192.168.17.116   | Yes              |
| MGH       | Yes / Yes                    | 192.168.17.117   | Yes              |
| UPHIE     | Yes / Yes                    | 192.168.17.118   | Yes              |

✅ *Zoom demo included showing successful pings from UPHIE to all hospitals.*

---

## Part 2: OpenEMR Installation and Security Hardening

**Objective**: Deploy OpenEMR on all 4 hospital VMs and secure them with industry best practices.

**Security Steps Implemented**:
- Ubuntu updates and firewall (UFW)
- Apache web server hardening
- Automatic security updates
- Custom strong passwords for OpenEMR users

**Tools Used**: Ubuntu Server, Apache2, OpenEMR v6.0.1  
**Remaining Risks Identified**: SQL injection, XSS (without proper sanitization)

---

## Part 3: Synthetic Patient and Syndromic Data Generation (Synthea)

**Objective**: Use Synthea to simulate a COVID-19 outbreak by generating HL7 FHIR data for each hospital.

| Hospital        | % Population Affected | COVID-19 Cases |
|-----------------|------------------------|----------------|
| Aspirus         | 0.4%                   | 20             |
| Portage Health  | 0.4%                   | 36             |
| BCMH            | 3%                     | 210            |
| MGH             | 6%                     | 1200           |

**Challenges**:
- Sorting JSON data by hospital required scripting
- Synthea setup on Windows (no `apt-get`) was non-trivial
- Manual calculation of population percentages for correct `-p` values

---

## Part 4: HAPI-FHIR Server Installation and Configuration

**Objective**: Deploy HAPI-FHIR on UPHIE VM using Docker for secure FHIR data ingestion.

**Tasks Completed**:
- Docker container setup for HAPI-FHIR with custom config
- UI successfully customized:
  - “Upper Peninsula Health Care Network”
  - “The Health Information Exchange for the Upper Peninsula”

**Commands Demonstrated**:
- Memory check: `free -h`
- Check open port: `sudo ufw status | grep 8080`
- Check if port is in use: `sudo lsof -i :8080`

---

## Part 5: Interoperability and HL7 FHIR Exchange via Postman

**Objective**: Simulate HL7 FHIR resource exchange using Postman

**Postman Demonstration**:
- `POST` request to create a Patient resource via HAPI-FHIR
- Status: `201 Created`
- Headers: `Content-Type: application/fhir+json`
- Successful connection to HAPI-FHIR endpoint (hosted on UPHIE VM)

**Key Q&A**:
- Used Basic Auth for testing
- Handled error codes and `OperationOutcome` responses via Postman
- Verified port access and server responses with custom scripts

---

## Tools & Technologies
- **EHR**: OpenEMR
- **FHIR Server**: HAPI-FHIR (Docker)
- **Simulation**: Synthea
- **Security**: UFW, Apache, OS hardening
- **Data Exchange**: HL7 FHIR (via Postman)
- **Programming/Scripting**: Bash, PowerShell

---

## Project Significance
This architecture models a scalable and interoperable disease surveillance system based on open-source health IT tools. It supports real-time outbreak response, public health policy creation, and regional collaboration among healthcare facilities — all while preserving patient privacy through synthetic data.

---

## Folder Structure in This Repo
