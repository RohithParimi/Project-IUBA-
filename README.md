# Project-IUBA
Intent-Aware User Behavior Analytics (IUBA)
# 🧠 Intent-Aware User Behavior Analytics (IUBA)

![Project Status](https://img.shields.io/badge/Status-Prototype-orange)
![Python](https://img.shields.io/badge/Python-3.9%2B-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Domain](https://img.shields.io/badge/Domain-Cybersecurity%20%7C%20ML-red)

> **A next-generation security intelligence layer that moves beyond static "Allow/Block" rules to understand the *Intent* behind user actions.**

---

## 📖 Table of Contents
- [Overview](#-overview)
- [The Problem](#-the-problem)
- [The Solution](#-the-solution)
- [System Architecture](#-system-architecture)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [Roadmap](#-roadmap)
- [Disclaimer](#-disclaimer)

---

## 📌 Overview

Traditional Endpoint Detection & Response (EDR) tools suffer from a **Semantic Gap**: they excel at detecting *what* happened (Action), but struggle to understand *why* it happened (Intent).

**IUBA** fills this gap. It is a machine-learning-driven system that analyzes user behavior patterns to classify activity into three distinct intent categories:
1.  **Benign/Business Use** (e.g., HR updating a PDF)
2.  **Admin/Maintenance** (e.g., SysAdmin patching a server)
3.  **Attacker/Malicious** (e.g., Privilege Escalation, Living-off-the-Land)

---

## 🎯 The Problem

1.  **The "Executive" Friction:** High-value, low-technical users (CEOs, Finance Heads) often trigger security blocks when performing benign actions (like updating Zoom), leading to operational bottlenecks.
2.  **The "Admin" Camouflage:** Attackers often abuse legitimate admin tools (PowerShell, PsExec). Traditional tools struggle to distinguish between a SysAdmin fixing a server and an attacker moving laterally.
3.  **Static Policies:** Hardcoded rules cannot adapt to context. A ban on "unknown installers" blocks malware, but also blocks legitimate productivity upgrades.

---

## 💡 The Solution

**Philosophy:** `Intent > Action > Enforcement`

Instead of judging a single log line in isolation, IUBA uses **Stateful Windowing** to analyze the *sequence* of actions over time.

| Action | Context | IUBA Verdict | Enforcement |
| :--- | :--- | :--- | :--- |
| `powershell.exe` | User is Admin, Weekend, mass file access | **Admin Abuse** | Alert + Verify |
| `powershell.exe` | User is HR, 2 AM, spawned by Word | **Attacker** | Block + Isolate |
| `setup.exe` | Signed by Adobe, standard path | **Legitimate Upgrade** | Allow + Log |

---

## 🏗 System Architecture

The system utilizes an ensemble of Machine Learning models to derive intent.

1.  **Data Ingestion:** Synthetic telemetry mimicking EDR logs (Process, Network, File).
2.  **Stateful Windowing:** Grouping logs by `Session_ID` (5-minute sliding windows).
3.  **The Model Core:**
    * **Model A (Non-IT):** *Isolation Forest* (Anomaly Detection) - Flags deviation from normal business workflows.
    * **Model B (Admin):** *XGBoost* (Classification) - Differentiates maintenance from abuse.
    * **Model C (Attacker):** *Sequence Analysis* - Detects Kill Chain patterns (Recon → PrivEsc).

---

## 📂 Project Structure

```bash
IUBA-Project/
├── data/
│   ├── synthetic_generator.py   # Script to generate fake EDR telemetry
│   ├── raw_logs.csv             # Generated dataset
│   └── trusted_vendors.json     # Allowlist logic
├── src/
│   ├── feature_engineering.py   # Converts text logs to numeric features
│   ├── anomaly_model.py         # Isolation Forest logic
│   └── dashboard.py             # Streamlit visualization
├── notebooks/                   # Jupyter notebooks for experimentation
├── requirements.txt             # Python dependencies
└── README.md                    # Project documentation
