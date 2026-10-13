# Modernization Plan: cloud-azure-modernization-plan

**Project**: Photo Album Java

---

## Technical Framework

- **Language**: Java 25
- **Framework**: Spring Boot 4.1.1
- **Build Tool**: Maven
- **Database**: Oracle Database (Oracle XE in the local Docker setup)
- **Key Dependencies**: Spring Data JPA, Hibernate, Thymeleaf, Spring Security

> The checked-in README contains older framework version details; the current
> Maven configuration is the source of truth. No framework upgrade is included.

---

## Overview

> This modernization prepares the Photo Album application in this repository
> for cloud operation and deployment on Azure. The application currently uses
> Oracle for photo and metadata storage and runs as a Spring Boot service. The
> new architecture will:
>
> - Move relational data from Oracle to Azure Database for PostgreSQL.
> - Provision Azure resources for the application and its database.
> - Deploy the application to Azure Container Apps.
> - Scan dependencies for known vulnerabilities and remediate findings.
>
> The plan establishes a source baseline, performs the database migration,
> verifies migrated behavior, completes security remediation, and then deploys.
> The request refers to multiple apps, but this repository contains one
> application; this plan covers Photo Album Java only.

---

## Migration Impact Summary

| App | Original | Azure target | Auth | Scope |
|-----|----------|--------------|------|-------|
| Photo Album | Oracle | Azure PostgreSQL | Managed identity | Database |
| Photo Album | Self-hosted | ACA | Managed identity | Hosting |

---

## Open Questions & Questionnaire

- [x] Q: Should the plan include environment/infrastructure provisioning? →
  A: Yes, provision new Azure infrastructure.
- [x] Q: Should the plan include integration testing? →
  A: Yes — Real mode, inferred because infrastructure will be provisioned.
- [x] Q: Should the plan include security/CVE remediation? →
  A: Yes, using the default dependency scan and remediation scope.
- [x] Q: Which Azure deployment target should be used? →
  A: Azure Container Apps, the default target.
- [ ] Azure subscription ID and preferred Azure region were not provided and
  must be supplied before provisioning.

---

## Infrastructure Plan

**User Requirements**: Provision Azure infrastructure for Photo Album Java,
migrate its Oracle database to PostgreSQL when present, and deploy the
application. The repository confirms one Java application and Oracle
configuration. Existing framework upgrade recommendations are out of scope.

**Plan Configuration**:

| Parameter | Value |
|-----------|-------|
| IaC Tool | Bicep |
| Provision | true |
| Subscription | Not provided |

### Proposed Architecture

```text
Photo Album Java
       |
       v
Azure Container Apps  --->  Azure Database for PostgreSQL
       |                         ^
       +---- Managed identity ---+
```

Infrastructure and deployment details, including resource naming, sizing, and
networking, are to be finalized during provisioning. No cost estimate is
included because pricing information was not available during plan generation.

### Azure Resource List

| Resource Type | Purpose |
|---------------|---------|
| Azure Container Apps environment and app | Host the application |
| Azure Container Registry | Store the application image |
| Azure Database for PostgreSQL Flexible Server | Replace Oracle |
| Log Analytics workspace | Collect application and platform logs |

---

## Security Compliance

Scan application dependencies for known CVEs and remediate identified
vulnerabilities before deployment. Upgrade vulnerable dependencies to the
minimum patched version; document any required major-version upgrade and its
breaking-change risk. Verify that the project builds and its tests pass after
remediation.
