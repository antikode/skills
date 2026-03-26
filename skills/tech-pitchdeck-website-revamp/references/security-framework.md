# Antikode Security Framework — Website Revamp

Antikode is ISO 27001 certified. All projects are built and operated under our Information Security Management System (ISMS). The following controls are standard across all web revamp projects.

---

## Security Controls by Area

### Application Security
- Secure coding practices enforced via SonarQube static analysis (SAST) in CI/CD pipeline
- Dependency vulnerability scanning on every build
- OWASP Top 10 mitigations applied by default (SQLi, XSS, CSRF, IDOR, etc.)
- Web application penetration testing with Burp Suite before go-live
- External penetration testing by independent third-party security firm
- Input validation and output encoding at all API boundaries
- Secrets management via environment variables — never hardcoded

### Infrastructure Security
- AWS Security Groups: principle of least privilege, no open 0.0.0.0/0 except ports 80/443
- SSH access restricted to Antikode ops IPs via key-pair authentication
- All S3 buckets private by default; public access only via pre-signed URLs or CloudFront
- AWS CloudTrail enabled for audit logging of all API actions
- Automatic OS patching schedule on all EC2/EKS instances

### Data Protection
- Data at rest: RDS encryption enabled (AES-256)
- Data in transit: TLS 1.2+ enforced on all endpoints; HSTS headers set
- S3 server-side encryption (SSE-S3 or SSE-KMS) for all stored objects
- Database backups automated with 7-day retention minimum (30-day for production)
- PII handling follows data minimization principles
- Migration data encrypted in transit and at rest during revamp cutover

### Access Management
- Admin panel access behind IP allowlist or VPN
- Role-based access control (RBAC) in Laravel CMS — no shared accounts
- MFA required for all admin accounts
- AWS IAM: separate roles per service, no root account usage in production
- Access reviews every 90 days

### Monitoring & Incident Response
- Uptime monitoring: Uptime Kuma (HTTP/TCP checks, alerting via Slack/email)
- Infrastructure metrics: Grafana + Prometheus/CloudWatch
- Log aggregation: Loki (application logs centralized and searchable)
- Error tracking: Glitchtip (exception monitoring with stack traces)
- Incident response runbook maintained per project
- SLA: P1 incidents acknowledged within 1 hour during business hours

### Compliance
- ISO 27001:2022 certified — covers information security management, risk assessment, and continuous improvement
- Regular internal audits and management reviews
- Supplier/vendor risk assessments for all third-party integrations
- All staff complete annual security awareness training
