# 🔐 Marconi Law Firm — Secure Web Infrastructure & Defense-in-Depth

![Linux](https://img.shields.io/badge/Linux-Administration-orange)
![Docker](https://img.shields.io/badge/Docker-Containerization-blue)
![Nginx](https://img.shields.io/badge/Nginx-Reverse%20Proxy-green)
![WordPress](https://img.shields.io/badge/WordPress-Web%20Application-blue)
![Cybersecurity](https://img.shields.io/badge/Focus-Cybersecurity-red)
![Defense in Depth](https://img.shields.io/badge/Security-Defense%20in%20Depth-purple)

## 📌 Project Overview

This project involved designing, deploying, and securing a multi-server web infrastructure for a fictional law firm, **Marconi Law Firm, LLC**.

The environment was designed as a proof-of-concept technical solution and cybersecurity audit documentation project. The infrastructure incorporated multiple Linux systems, Docker, a Ghost application, an Nginx reverse proxy, a WordPress/LAMP stack, SSH remote administration, network configuration, and application-level security controls.

The project followed a **Defense-in-Depth** approach by implementing security controls at multiple layers of the environment rather than relying on a single security mechanism.

### Primary Security Objectives

- Establish a segmented custom network environment
- Enable secure remote administration through SSH
- Deploy applications using Docker
- Configure Nginx as a reverse proxy
- Deploy a WordPress/LAMP web application
- Identify and remediate insecure file permissions
- Protect sensitive WordPress configuration files
- Implement application-level firewall/security controls
- Validate security configurations
- Document the infrastructure and security controls

---

# 🏗️ Lab Architecture

The environment consisted of multiple virtual machines connected through a custom network.

```text
                         ┌─────────────────────┐
                         │   Custom Firewall   │
                         │      / Router       │
                         │    10.10.229.1      │
                         └──────────┬──────────┘
                                    │
                         ITE229 Network
                         10.10.229.0/24
                                    │
              ┌─────────────────────┼─────────────────────┐
              │                     │                     │
              ▼                     ▼                     ▼
     ┌────────────────┐    ┌────────────────┐    ┌────────────────┐
     │  Rocky Linux 8 │    │  Rocky Linux 8 │    │ Ubuntu Linux   │
     │  Docker Host   │    │ Nginx Reverse  │    │ WordPress/LAMP │
     │ 10.10.229.11   │    │     Proxy      │    │ 10.10.229.12   │
     │                │    │ 10.10.229.10   │    │                │
     │ Ghost / Docker │───▶│     Nginx      │    │ Apache2        │
     │                │    │                │    │ MySQL          │
     └────────────────┘    └────────────────┘    │ PHP            │
                                                  │ WordPress      │
                                                  └────────────────┘
````

---

# 🌐 Network Configuration

| Component         | Operating System | Role             | IP Address     |
| ----------------- | ---------------- | ---------------- | -------------- |
| Firewall / Router | Virtual Firewall | Network Gateway  | `10.10.229.1`  |
| Docker Server     | Rocky Linux 8    | Ghost / Docker   | `10.10.229.11` |
| Nginx Server      | Rocky Linux 8    | Reverse Proxy    | `10.10.229.10` |
| WordPress Server  | Ubuntu Linux     | LAMP / WordPress | `10.10.229.12` |

### Custom Network

| Setting         | Configuration   |
| --------------- | --------------- |
| Network Name    | `ITE229`        |
| Network Address | `10.10.229.0`   |
| Subnet Mask     | `255.255.255.0` |
| Gateway         | `10.10.229.1`   |
| DNS             | `10.10.229.1`   |

---

# 🧰 Technologies & Tools

## Infrastructure

* Rocky Linux 8
* Ubuntu Linux
* Virtual Machines
* Custom Network Configuration

## Remote Administration

* OpenSSH
* SSH Client
* Linux Command Line

## Containerization

* Docker CE
* Docker Engine
* Docker Compose
* Ghost Container

## Web Infrastructure

* Nginx
* Apache2
* PHP
* MySQL
* WordPress
* Ghost

## Security

* Firewall Configuration
* File Permissions
* Linux Access Controls
* WordPress Security
* Reverse Proxy Architecture
* Defense-in-Depth
* Security Validation
* Access and Error Logging

---

# 🔑 1. Secure Remote Administration

SSH was configured to allow remote administration of the Linux systems.

The Ubuntu system was configured with the OpenSSH client:

```bash
sudo apt install openssh-client -y
```

SSH provided remote access for system administration and configuration without requiring direct console access to each virtual machine.

### Security Relevance

Remote administration introduces an important security boundary.

SSH should be protected through controls such as:

* Strong authentication
* Key-based authentication
* Restricted administrative access
* Firewall rules
* Monitoring and logging
* Least-privilege administration

---

# 🐳 2. Docker Infrastructure

A Rocky Linux 8 system was configured as the Docker host.

The system was updated and prepared for Docker:

```bash
sudo yum update -y
```

The EPEL repository was installed:

```bash
sudo yum install epel-release -y
```

Docker's stable repository was configured and Docker CE was installed.

```bash
sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Docker installation was verified using:

```bash
docker -v
```

---

# ⚙️ 3. Initialize Docker

Docker was started and configured to launch automatically:

```bash
sudo systemctl start docker
```

```bash
sudo systemctl enable docker
```

The installation was tested with:

```bash
sudo docker run hello-world
```

This confirmed that the Docker engine was operational and capable of running containers.

---

# 👻 4. Deploy Ghost Using Docker

A Ghost container was deployed on the Rocky Linux Docker host.

The Docker environment provided an isolated application platform for the Ghost website.

The container deployment demonstrated:

* Containerized application deployment
* Docker administration
* Application isolation
* Linux system administration
* Service lifecycle management

The Ghost container was also validated using Docker commands such as:

```bash
docker ps
```

---

# ⚠️ 5. SELinux Configuration

SELinux was disabled during the lab configuration to avoid compatibility issues with the assigned environment.

> **Security Note:** Disabling SELinux reduces a significant Linux security control and would generally **not** be recommended for a production deployment.

A production implementation should instead investigate compatibility issues and maintain SELinux in enforcing mode whenever practical.

This is an important lesson from the project because a configuration that simplifies deployment can also weaken the overall security posture.

---

# 🔀 6. Nginx Reverse Proxy

A second Rocky Linux 8 server was configured as the Nginx reverse proxy.

Nginx was installed and configured to route web traffic toward the backend application.

```text
Internet / Client
       │
       ▼
    Nginx
Reverse Proxy
       │
       ▼
Backend Application
       │
       ▼
     Ghost
```

The reverse proxy architecture provides an additional layer between external clients and backend application services.

### Security Benefits

A reverse proxy can provide opportunities for:

* Traffic control
* Request filtering
* Centralized logging
* TLS termination
* Application isolation
* Backend service protection

---

# 🌐 7. Nginx Configuration

Nginx was configured to act as a reverse proxy for the Ghost application.

After modifying the configuration, the Nginx service was reloaded to apply the changes.

The configuration was then validated by accessing the Ghost application through the Nginx server.

The project also documented Nginx access and error logs for troubleshooting and security monitoring.

---

# 🐧 8. Ubuntu LAMP Stack

A separate Ubuntu server was configured as the WordPress web server.

The LAMP environment consisted of:

```text
Linux
Apache
MySQL
PHP
```

The Apache web server was installed and tested before the remaining components were configured.

The environment was then expanded with:

* MySQL
* PHP
* Required PHP libraries
* Required MySQL libraries
* Apache URL rewriting
* WordPress

---

# 🗄️ 9. MySQL Database Configuration

MySQL was configured to support the WordPress application.

Database administration included:

* MySQL installation
* Root account configuration
* Privilege management
* WordPress database configuration
* Database user configuration

### Security Considerations

Database security is critical because WordPress stores application information and configuration data within the database.

Production environments should additionally implement:

* Strong unique passwords
* Least-privilege database accounts
* Restricted database network access
* Regular backups
* Database monitoring
* Secure credential management

---

# 🌐 10. WordPress Deployment

WordPress was deployed on the Ubuntu LAMP server.

The WordPress application was configured to operate through Apache, PHP, and MySQL.

The completed website was tested through the configured network environment.

### Application Stack

```text
WordPress
     │
     ▼
   PHP
     │
     ▼
  Apache
     │
     ▼
   Linux
     │
     ▼
  Network
```

---

# 🔍 11. WordPress File Permission Assessment

The WordPress directory was inspected using:

```bash
cd /var/www/html/
ls -la
```

The assessment identified overly permissive file permissions, including permissions associated with `wp-config.php`.

This represented a potential security issue because sensitive configuration files should not be unnecessarily accessible to other users.

---

# 🔒 12. Harden WordPress File Permissions

Directory permissions were changed using:

```bash
sudo find /var/www/html/* -type d -exec chmod 750 {} \;
```

File permissions were changed using:

```bash
sudo find /var/www/html/* -type f -exec chmod 640 {} \;
```

The goal was to reduce unnecessary access to WordPress files and directories.

### Security Principle

This configuration follows the principle of **least privilege**.

Users and processes should only receive the permissions required to perform their intended functions.

---

# 🛡️ 13. Protect wp-config.php

The WordPress configuration file contains sensitive application configuration information and therefore requires additional protection.

The file was moved outside the primary web root:

```bash
mv wp-config.php /var/www/
```

This reduced the risk of the configuration file being directly exposed through the web application directory.

### Security Relevance

Protecting configuration files is important because they can contain:

* Database credentials
* Database host information
* Authentication salts
* Application configuration
* Other sensitive settings

---

# 🧱 14. WordPress Application Firewall

A WordPress security/firewall plugin was configured to provide additional application-level security controls.

The project evaluated WordPress firewall options and configured a strong security profile.

The security layer was intended to provide proactive protection for the WordPress application.

### Defense-in-Depth Model

```text
Network
   │
   ▼
Firewall / Router
   │
   ▼
Reverse Proxy
   │
   ▼
Web Server
   │
   ▼
WordPress Security Layer
   │
   ▼
Application
   │
   ▼
Database
```

Each layer provides an additional opportunity to prevent, detect, or limit attacks.

---

# 🧪 15. Security Validation

The project incorporated validation steps after configuration changes.

Validation included:

* Verifying Docker installation
* Verifying running containers
* Confirming Nginx operation
* Testing the reverse proxy
* Testing the WordPress website
* Reviewing file permissions
* Verifying protected WordPress configuration
* Confirming application security controls

Validation is an important component of secure configuration because a change should not be considered successful until its intended result has been verified.

---

# 📋 Security Assessment Summary

| Area                 | Assessment      | Security Objective           |
| -------------------- | --------------- | ---------------------------- |
| SSH                  | Configured      | Remote Administration        |
| Docker               | Deployed        | Application Isolation        |
| Ghost                | Containerized   | Application Deployment       |
| Nginx                | Reverse Proxy   | Traffic Control              |
| Apache               | Web Server      | Application Hosting          |
| MySQL                | Database        | Data Storage                 |
| WordPress            | Web Application | Business Application         |
| File Permissions     | Hardened        | Least Privilege              |
| `wp-config.php`      | Relocated       | Configuration Protection     |
| Application Firewall | Configured      | Web Application Protection   |
| Logging              | Documented      | Monitoring / Troubleshooting |
| Validation           | Performed       | Configuration Assurance      |

---

# 🛡️ Defense-in-Depth Strategy

The project implemented security at multiple layers instead of relying on a single control.

### Layer 1 — Network

The custom network and firewall/router provided the initial network boundary.

### Layer 2 — Remote Administration

SSH enabled remote administration while creating an additional authentication and access-control boundary.

### Layer 3 — Reverse Proxy

Nginx separated external web traffic from backend application services.

### Layer 4 — Server

Linux systems hosted the individual infrastructure components.

### Layer 5 — Application

WordPress and Ghost were deployed as separate applications.

### Layer 6 — File Permissions

Linux permissions restricted access to WordPress files and directories.

### Layer 7 — Configuration Protection

`wp-config.php` was moved outside the web root to provide additional protection for sensitive configuration information.

### Layer 8 — Application Security

A WordPress security/firewall layer was implemented to provide additional protection at the application level.

---

# 🔐 Security Lessons Learned

One of the most important lessons from this project was that **security is not a single configuration setting**.

A secure infrastructure requires multiple controls working together.

The project demonstrated how network controls, remote-access security, reverse proxies, server configuration, file permissions, application security, and monitoring can work together as part of a Defense-in-Depth strategy.

The project also demonstrated that security controls can introduce tradeoffs.

For example, disabling SELinux simplified the lab deployment but removed an important Linux security control. A production environment should avoid weakening security controls simply to make deployment easier.

---

# ⚠️ Security Improvements for a Production Deployment

The original lab environment contains several configurations that should be improved before being considered production-ready.

## SELinux

Re-enable SELinux and investigate compatibility issues rather than leaving it disabled.

## Firewall

Maintain host-based firewall controls instead of disabling them for convenience.

## SSH

Implement:

* SSH key authentication
* Disable password authentication where appropriate
* Disable direct root login
* Restrict SSH access
* Monitor authentication attempts

## TLS / HTTPS

Configure HTTPS using modern TLS settings and valid certificates.

## Docker

Implement:

* Minimal container images
* Non-root containers where possible
* Image vulnerability scanning
* Container resource limits
* Controlled image sources
* Regular image updates

## WordPress

Implement:

* Regular updates
* Plugin security reviews
* Strong authentication
* MFA
* Least-privilege administrative accounts
* Secure backups
* File integrity monitoring

## Database

Implement:

* Least-privilege database accounts
* Strong credential management
* Restricted database exposure
* Regular backups
* Database monitoring

## Monitoring

Integrate infrastructure logs with a centralized SIEM such as:

* Splunk
* Elastic Security
* Microsoft Sentinel

---

# 🚀 Future Improvements

To expand this project into a more advanced cybersecurity lab, I would implement:

* [ ] Re-enable SELinux
* [ ] Harden SSH configuration
* [ ] Implement SSH key authentication
* [ ] Harden Linux firewalls
* [ ] Configure HTTPS/TLS
* [ ] Add centralized log collection
* [ ] Integrate logs with Splunk
* [ ] Perform Nmap network discovery
* [ ] Perform Nessus vulnerability scanning
* [ ] Scan Docker images for vulnerabilities
* [ ] Implement container security policies
* [ ] Implement WordPress MFA
* [ ] Implement automated backups
* [ ] Add file-integrity monitoring
* [ ] Perform web application vulnerability testing
* [ ] Document security findings and remediation
* [ ] Create a final security assessment report

---

# 📊 Skills Demonstrated

### Linux Administration

* Rocky Linux 8
* Ubuntu Linux
* Package management
* Service management
* File permissions
* Linux command line
* System configuration

### Networking

* IPv4 networking
* Custom network configuration
* Network gateways
* Reverse proxy architecture
* Firewall concepts
* Network segmentation

### Web Infrastructure

* Nginx
* Apache
* PHP
* MySQL
* WordPress
* Ghost

### Containerization

* Docker CE
* Docker Engine
* Docker containers
* Containerized application deployment

### Cybersecurity

* Defense-in-Depth
* Least privilege
* Attack surface reduction
* Secure configuration
* File permission hardening
* Application security
* Security validation
* Security logging
* Vulnerability management concepts

---

# 🧠 Key Takeaways

This project provided practical experience designing and managing a multi-server Linux web environment while applying cybersecurity principles throughout the deployment process.

The biggest takeaway was the importance of **layered security**.

Instead of treating the web application as the only security concern, the project considered security across the network, operating system, remote administration, reverse proxy, web server, application, filesystem, and database layers.

This project also strengthened my understanding of how infrastructure decisions affect cybersecurity. Configuration choices such as firewall management, SELinux, file permissions, application deployment, and reverse proxy architecture can directly influence an organization's attack surface and overall security posture.

# 🎯 Portfolio Relevance

This project demonstrates skills applicable to entry-level cybersecurity and infrastructure security roles, including:

* SOC Analyst
* Security Analyst
* Junior Security Engineer
* Systems Security Administrator
* Vulnerability Management Analyst
* Network Security Analyst

The project demonstrates that I can work with both **infrastructure and security controls**, understand how systems interact across a network, identify configuration weaknesses, and apply security improvements.

---

## 👤 Author

### Sean Redding

**Cybersecurity | Security Operations | Vulnerability Management | Security Engineering**

* 🌐 **GitHub:** [https://github.com/SealT6](https://github.com/SealT6)
* 💼 **LinkedIn:** [https://www.linkedin.com/in/sean-redding-aa503a293/](https://www.linkedin.com/in/sean-redding-aa503a293/)
