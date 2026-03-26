# Antikode AWS Topology Tiers

Three standard infrastructure tiers for Antikode web projects. Choose based on complexity classification.

---

## Tier 1 — Simple

**Use for:** Brochure sites, landing pages, low-traffic company sites, blogs. No e-commerce. Single-region, no redundancy required.

| Component | AWS Spec | Est. Monthly Cost |
|---|---|---|
| Web Server (FE + CMS combined) | 1× EC2 t3.small (2 vCPU, 2 GB RAM) | $23.70 |
| Storage | S3 Standard (media/assets) | Included in estimate |
| **Total** | | **$23.70/mo** |

- **Capacity:** ~50 concurrent users/sec
- **Notes:** Single EC2 instance running both Next.js and Laravel. No load balancer. Suitable for < 10k monthly visits. Add RDS separately if DB isolation is needed (adds ~$15–20/mo for db.t3.micro).

---

## Tier 2 — Medium

**Use for:** Marketing sites with campaigns, content-heavy sites, simple e-commerce, portals with user accounts. Moderate traffic with occasional spikes.

| Component | AWS Spec | Est. Monthly Cost |
|---|---|---|
| Frontend Servers | 2× EC2 t3.medium (Next.js) | $60.00 |
| CMS Server | 1× EC2 t3.medium (Laravel) | $30.00 |
| Database | 1× RDS db.t3.medium (MySQL) | $29.00 |
| Load Balancer | 1× AWS ALB | $8.50 |
| Storage | S3 Standard | $6.00 |
| **Total** | | **$133.50/mo** |

- **Capacity:** 200–300 concurrent users/sec
- **Notes:** Separate FE and CMS layers. ALB distributes FE traffic. RDS for managed DB with automated backups. Add ElastiCache t3.micro for Redis if needed (+~$15/mo). Suitable for up to ~100k monthly visits.

---

## Tier 3 — Advanced

**Use for:** E-commerce platforms, high-traffic portals, apps with complex integrations, campaigns with flash-sale peaks.

| Component | AWS Spec | Est. Monthly Cost |
|---|---|---|
| Frontend Servers | 2× EC2 t3.large (Next.js) | $60.48 |
| CMS Servers | 2× EC2 t3.large (Laravel) | $60.48 |
| Database | 1× RDS db.t3.large (MySQL/PostgreSQL) | $29.00 |
| Load Balancers | 2× AWS ALB (FE + CMS) | $17.00 |
| Storage | S3 Standard | $4.00 |
| **Total** | | **$170.94/mo** |

- **Capacity:** 350–400 concurrent users/sec
- **Notes:** Fully redundant FE and CMS layers with dedicated ALBs. RDS with Multi-AZ option available (+~$29/mo for HA). Redis ElastiCache recommended (adds ~$15/mo). CloudFront CDN layer recommended for global clients (adds ~$5–20/mo depending on data transfer).

---

## Notes on All Tiers

- Costs are estimates based on ap-southeast-1 (Singapore) pricing as of 2025. Actual costs may vary.
- All tiers include S3 for media/static asset storage.
- Add Route 53 (~$0.50/hosted zone + query costs) for DNS management.
- SSL/TLS via AWS Certificate Manager (free with ALB/CloudFront).
- Monitoring stack (Uptime Kuma, Grafana, Loki, Glitchtip) deployed on a shared ops EC2 instance — not billed per project.
