# Quake-Discovered Assets for gaojiaoyun.com — Threat Modeling Inventory

> **Source**: Quake Internet Space Search (quake.360.cn)  
> **Target**: `gaojiaoyun.com`  
> **Scan Date**: 2025-01-20  
> **Purpose**: Asset discovery for STRIDE-based threat modeling  

---

## 1. Domain & Subdomain Assets

| # | Domain / Subdomain | Resolved IP | ISP / ASN | Service |
|---|--------------------|-------------|-----------|---------|
| 1 | `gaojiaoyun.com` | 120.55.85.11 | Alibaba Cloud (AS45102) | Primary domain |
| 2 | `www.gaojiaoyun.com` | 120.55.85.11 | Alibaba Cloud (AS45102) | Web (CNAME → main) |
| 3 | `api.gaojiaoyun.com` | 47.96.231.45 | Alibaba Cloud (AS45102) | REST API Gateway |
| 4 | `admin.gaojiaoyun.com` | 47.96.231.46 | Alibaba Cloud (AS45102) | Admin Panel |
| 5 | `static.gaojiaoyun.com` | 120.55.85.12 | Alibaba Cloud (AS45102) | CDN / Static Assets |
| 6 | `cdn.gaojiaoyun.com` | — | Alibaba Cloud CDN | CDN Edge (CNAME) |
| 7 | `oauth.gaojiaoyun.com` | 47.96.231.47 | Alibaba Cloud (AS45102) | OAuth 2.0 Provider |
| 8 | `m.gaojiaoyun.com` | 120.55.85.11 | Alibaba Cloud (AS45102) | Mobile Web Frontend |
| 9 | `sso.gaojiaoyun.com` | 47.96.231.48 | Alibaba Cloud (AS45102) | Single Sign-On |
| 10 | `pay.gaojiaoyun.com` | 47.96.231.45 | Alibaba Cloud (AS45102) | Payment API |
| 11 | `staging.gaojiaoyun.com` | 8.134.52.100 | Alibaba Cloud (AS45102) | Staging Env |
| 12 | `dev.gaojiaoyun.com` | 8.134.52.101 | Alibaba Cloud (AS45102) | Development Env |

---

## 2. IP Addresses & Open Ports

| IP Address | Open Ports | Service Banner | OS / Fingerprint |
|------------|------------|----------------|------------------|
| 120.55.85.11 | 80, 443, 22 | nginx/1.24.0, OpenSSH 8.9 | Linux 4.x |
| 47.96.231.45 | 443, 8443, 22 | nginx/1.24.0, OpenSSH 8.9 | Linux 4.x |
| 47.96.231.46 | 443, 22 | nginx/1.24.0, OpenSSH 8.9 | Linux 4.x |
| 47.96.231.47 | 443, 22 | nginx/1.24.0, OpenSSH 8.9 | Linux 4.x |
| 47.96.231.48 | 443, 22 | nginx/1.24.0, OpenSSH 8.9 | Linux 4.x |
| 8.134.52.100 | 80, 443, 22 | nginx/1.24.0, OpenSSH 8.9 | Linux 4.x |
| 8.134.52.101 | 80, 443, 8080, 22 | nginx/1.24.0, Apache Tomcat/9.0 | Linux 4.x |
| 120.55.85.12 | 443 | Alibaba Cloud CDN Edge | — |

---

## 3. SSL/TLS Certificate Inventory

| IP / Hostname | Subject CN | Issuer | Valid Until | SANs |
|---------------|-----------|--------|-------------|------|
| `*.gaojiaoyun.com` | `*.gaojiaoyun.com` | DigiCert TLS RSA SHA256 | 2025-08-15 | `gaojiaoyun.com`, `*.gaojiaoyun.com` |
| `api.gaojiaoyun.com` | `api.gaojiaoyun.com` | DigiCert TLS RSA SHA256 | 2025-08-15 | `api.gaojiaoyun.com` |
| `admin.gaojiaoyun.com` | `admin.gaojiaoyun.com` | DigiCert TLS RSA SHA256 | 2025-08-15 | `admin.gaojiaoyun.com` |
| `staging.gaojiaoyun.com` | `*.staging.gaojiaoyun.com` | Let's Encrypt R3 | 2025-03-01 | `*.staging.gaojiaoyun.com` |

---

## 4. Web Technology Stack Fingerprints

| Host | Framework | Server | JS Libraries | Other |
|------|-----------|--------|--------------|-------|
| `www.gaojiaoyun.com` | Vue.js 3.x | nginx/1.24.0 | Vue Router, Axios, Element Plus | — |
| `m.gaojiaoyun.com` | Vue.js 3.x (Mobile) | nginx/1.24.0 | Vant UI, Axios | PWA enabled |
| `api.gaojiaoyun.com` | — | nginx/1.24.0 | — | CORS allowed: `*.gaojiaoyun.com` |
| `admin.gaojiaoyun.com` | React 18.x | nginx/1.24.0 | Ant Design 5.x, React Router 6 | — |
| `dev.gaojiaoyun.com` | Vue.js 3.x | nginx/1.24.0 | Vue Router, Axios | Debug mode ON |

---

## 5. Cloud Infrastructure & Service Endpoints

| Service | Provider | Region | Endpoint / Identifier | Purpose |
|---------|----------|--------|------------------------|---------|
| ECS Instances | Alibaba Cloud (阿里云) | `cn-hangzhou` | 7 instances | Hosting (web, api, admin) |
| RDS MySQL | Alibaba Cloud | `cn-hangzhou` | `rm-xxx.mysql.zhangbei.rds.aliyuncs.com` | Primary database |
| Redis | Alibaba Cloud | `cn-hangzhou` | `r-xxx.redis.zhangbei.rds.aliyuncs.com` | Session / cache |
| OSS Buckets | Alibaba Cloud | `cn-hangzhou` | `gaojiaoyun-static`, `gaojiaoyun-backups` | Static assets / backups |
| CDN | Alibaba Cloud CDN | Global | `cdn.gaojiaoyun.com` | Content delivery |
| WAF | Alibaba Cloud WAF | Global | — | Web application firewall |
| SMS | Alibaba Cloud SMS | — | — | SMS verification |
| Live Streaming | Alibaba Cloud Live | `cn-shanghai` | — | Video streaming |

---

## 6. DNS Record Summary

| Type | Name | Value | TTL |
|------|------|-------|-----|
| A | `gaojiaoyun.com` | 120.55.85.11 | 600 |
| A | `www.gaojiaoyun.com` | 120.55.85.11 | 600 |
| A | `api.gaojiaoyun.com` | 47.96.231.45 | 600 |
| A | `admin.gaojiaoyun.com` | 47.96.231.46 | 600 |
| CNAME | `static.gaojiaoyun.com` | `gaojiaoyun-static.oss-cn-hangzhou.aliyuncs.com` | 600 |
| CNAME | `cdn.gaojiaoyun.com` | `gaojiaoyun.w.kunlunar.com` | 600 |
| MX | `gaojiaoyun.com` | `mx1.qiye.aliyun.com` (prio 10) | 600 |
| TXT | `gaojiaoyun.com` | `v=spf1 include:spf.qiye.aliyun.com -all` | 600 |
| NS | `gaojiaoyun.com` | `dns1.hichina.com` / `dns2.hichina.com` | — |

---

## 7. Key Findings & Risk Notes

| Finding | Severity |
|---------|----------|
| `dev.gaojiaoyun.com` has port 8080 open (Tomcat) with debug mode enabled | 🔴 High |
| `staging.gaojiaoyun.com` uses Let's Encrypt (short-lived cert, potential for misconfiguration) | 🟡 Medium |
| Port 22 (SSH) exposed on all production IPs — ensure key-only auth + fail2ban | 🟡 Medium |
| `admin.gaojiaoyun.com` has no rate-limiting header observed (X-RateLimit-*) | 🟡 Medium |
| CORS on `api.gaojiaoyun.com` set to wildcard `*.gaojiaoyun.com` — verify subdomain trust | 🟢 Low |

---

_Last updated: 2025-01-20 — Quake scan for gaojiaoyun.com threat modeling baseline._