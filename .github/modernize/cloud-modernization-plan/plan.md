# Modernization Plan: Cloud Modernization Plan

**Project**: Photo Album (Java)

---

## Technical Framework

- **Language**: Java 25
- **Framework**: Spring Boot 4.1.1
- **Build Tool**: Maven
- **Database**: Oracle Database (current); Azure Database for PostgreSQL (target)
- **Key Dependencies**: Spring Data JPA, Hibernate, Thymeleaf

The versions above are based on the current Maven project configuration. No
framework or Java upgrade is included; the existing assessment report is not
present in this repository, and upgrade recommendations are out of scope.

---

## Overview

> This modernization moves the Photo Album application from its Oracle-backed
> local container setup to Azure. The application currently stores photo data
> in Oracle and is packaged as a Spring Boot application. The new architecture
> will:
>
> - Move relational data to Azure Database for PostgreSQL, including replacing
>   Oracle-specific database behavior.
> - Provision the Azure resources needed to host the application and database.
> - Deploy the application to Azure Container Apps.
>
> The work proceeds through database migration and cloud resource provisioning,
> then verifies the migrated service and deploys the application.

---

## Migration Impact Summary

| App | Source | Azure target | Authentication | Notes |
|-----|--------|--------------|----------------|-------|
| Photo Album | Oracle DB | Azure PostgreSQL | Managed identity | Replace Oracle SQL |
| Photo Album | Local Docker | Container Apps | Managed identity | Cloud application host |

---

## Azure Infrastructure

**User requirements**: Resolve cloud-readiness issues, provision Azure
infrastructure for the application, and deploy it. The repository contains one
application (Photo Album); no second application or assessment report is
available in this checkout.

**Plan configuration**:

| Parameter | Value |
|-----------|-------|
| IaC tool | Bicep |
| Provision resources | Yes |
| Subscription | Not provided |
| Deployment target | Azure Container Apps |

**Proposed architecture**:

```text
Build pipeline --> Azure Container Registry --> Photo Album container app
                                               | (Azure Container Apps)
                                               | managed identity
                                               v
                                      Azure Database for PostgreSQL
Users --------------------------------> Photo Album container app
```

**Azure resource list**:

| Resource | Purpose |
|----------|---------|
| Azure Container Apps environment and app | Host the Photo Album application |
| Azure Container Registry | Store the application container image |
| Azure Database for PostgreSQL Flexible Server | Persist application metadata and photos |
| User-assigned or system-assigned managed identity | Enable passwordless resource access |

Resource region, sizing, and monthly cost remain to be determined during
infrastructure planning; no subscription or pricing data was provided.

---

## Security Compliance

Scan project dependencies for known CVEs after database transformation and
remediate identified vulnerabilities before deployment. Upgrade vulnerable
dependencies to minimum patched versions; document breaking-change risks when
a major-version upgrade is required, and verify the build and unit tests.

---

## Open Questions & Questionnaire

- [x] Environment setup → Provision new Azure infrastructure, as requested.
- [ ] Azure subscription ID → Not provided; required before provisioning.
- [x] Integration testing → Include real-mode verification against provisioned
  Azure infrastructure (the default when infrastructure is provisioned).
- [x] Security/CVE remediation → Include the default security task.
- [x] Deployment target → Azure Container Apps (default).
- [ ] Second application → Only Photo Album Java is present in this repository;
  provide the second application's repository or source before it can be
  included in this plan.
- [ ] Assessment report → No assessment report is present in this checkout;
  cloud-readiness scope is based on the current application configuration and
  Oracle-specific repository code.
