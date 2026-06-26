# automated-iac-web-monitoring
#Automated Enterprise Web Server Deployment and High-Fidelity Real-time Infrastructure Monitoring stack using Ansible, Prometheus, and Grafana on RHEL 9.
# Automated Web Deployment & Infrastructure Monitoring via IaC

📌 Project Overview
This project architecture blueprint establishes an automated, production-ready enterprise web infrastructure and multi-tier observability ecosystem deployed on a hardened Red Hat Enterprise Linux (RHEL) 9 architecture. By treating core system infrastructure completely as version-controlled software configurations, this deployment pipeline successfully mitigates structural provisioning latency, manual human-error factors, and environment state drift.

The implementation relies on an agentless Infrastructure as Code (IaC) push execution engine for rapid web clustering, tightly integrated with a pull-based, time-series telemetry stack to maintain absolute observability over system resource metrics 24/7.

🏗️ Distributed Cluster Topology
The internal private virtual network fabric is mapped programmatically across discrete node layers:
*   Ansible Control Node (Management Hub): 192.168.1.10 | Core administration server orchestrating target node configurations via stateful Python execution primitives over secure OpenSSH channels.
*   Target Web Workstation (Application Node): 192.168.1.20 | Managed node hosting the live Apache HTTP daemon alongside decoupled kernel-level metric gathering exporters.
*   Centralized Monitoring Station (Observability Console): 192.168.1.30 | Standalone node running dedicated database time-series metric scrapers and visual dynamic analytic metric gateways.

⚙️ Core Automation Logic & IaC Architecture
The configuration states are synchronized uniformly from the Control Hub using Asymmetric Cryptography (RSA 4096-bit authentication keys) to enforce strict password-less machine communication channels. 

The primary configuration logic guarantees strict task idempotency through specific YAML playbook definitions:
*   Automated Package Orchestration: Leverages native RHEL package manager (DNF/YUM) directives to query, install, and update required application binaries safely.
*   Persistent Systemd Configuration: Enforces long-term infrastructure service durability by explicitly initializing runtime and boot-time process parameters for the system daemons.
*   Dynamic Content Delivery: Deploys the static landing page artifacts directly into host document root contexts (`/var/www/html/index.html`) while managing systemic permissions natively.
*   Perimeter Hardening Automation: Interacts directly with the stateful Firewalld subsystem to dynamically whitelist essential ingress points (Port 80) while verifying a strict default drop policy for external network probes.

📈 High-Fidelity Time-Series Observability Framework
The monitoring architecture is completely decoupled from public web presentation pathways to prevent cross-service resource starvation and maintain structural data transparency:

1. Time-Series Metrics Ingestion (Prometheus TSDB)
*   Implements an internal HTTP Pull-Model to execute telemetry scraping requests to remote target endpoints at optimized 15-second intervals.
*   Tracks kernel-level execution states through Node Exporter hooks running on Port 9100 and tracks specialized web process statuses via Apache Metrics Exporters on Port 9117.
*   Provides low-latency diagnostic capabilities, instantly recording time-series metrics under explicit operational labels for historical trend analysis.

2. Analytics Visualization Gateway (Grafana Server)
*   Acts as the central dashboard hub, sending automated PromQL (Prometheus Query Language) statements to the Prometheus data backend API on Port 9090.
*   Translates continuous floats and multi-dimensional metrics into high-fidelity, human-readable monitoring grids mapping core resource allocations.
*   Tracks the "Four Golden Signals" of system reliability: Latency, Traffic, Process Errors, and Hardware Resource Saturation (CPU overhead, RAM page utilization, Disk I/O).
