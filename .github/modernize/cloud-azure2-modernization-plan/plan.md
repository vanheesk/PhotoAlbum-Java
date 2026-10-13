# Modernization Plan: Photo Album Azure Cloud Modernization

**Project**: Photo Album

---

## Technical Framework

- **Language**: Java 8
- **Framework**: Spring Boot 2.7.18
- **Build Tool**: Maven
- **Database**: Oracle Database 21c Express Edition
- **Key Dependencies**: Spring MVC, Thymeleaf, Spring Data JPA, Hibernate, Spring Security

---

## Overview

> This modernization prepares the Photo Album application for operation in Azure. The application currently runs as a containerized Spring Boot web application backed by Oracle Database and stores uploaded photos in database BLOBs. The new architecture will:
>
> - Replace Oracle with Azure Database for PostgreSQL while retaining photo and metadata persistence.
> - Address the application's cloud-readiness needs and provision the Azure resources required to run it.
> - Containerize, deploy, and verify the application on Azure Kubernetes Service.
>
> The work will establish a baseline, migrate the database and address cloud readiness, provision Azure infrastructure, verify migrated services, then deploy the application and complete a security review. Existing framework versions are retained; framework-upgrade recommendations are out of scope.

---

## Migration Impact Summary

| Application | Original Service | New Azure Service | Authentication | Comments |
|-------------|------------------|-------------------|----------------|----------|
| Photo Album | Oracle Database | Azure Database for PostgreSQL | Managed identity | Migrate stored photos and metadata |
| Photo Album | Local container deployment | Azure Kubernetes Service | Managed identity | Extend the repository's AKS deployment setup |

---

## Open Questions & Questionnaire

- [x] Environment setup → Provision new Azure infrastructure, as requested.
- [x] Integration testing → Include real-mode integration tests against the provisioned Azure environment, inferred from the infrastructure request.
- [x] Security/CVE remediation → Include the required dependency security review.
- [x] Deployment target → Azure Kubernetes Service, inferred from the existing `azure-setup.ps1` AKS configuration.
- [x] Containerization → Included in deployment; no separate containerization task is needed.
- [ ] Azure subscription was not provided; select it before provisioning. The repository's existing setup defaults to `westus3`.
- [ ] No assessment report was available in this repository. Only the Oracle-to-PostgreSQL request and cloud-readiness scope stated in the issue are included; framework upgrades are excluded.
