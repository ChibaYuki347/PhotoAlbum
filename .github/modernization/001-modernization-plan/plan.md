# Modernization Plan: Oracle Database to Azure Database for PostgreSQL Migration

**Project**: Photo Album Application  

---

## Technical Framework

- **Language**: Java 8
- **Framework**: Spring Boot 2.7.18
- **Build Tool**: Maven
- **Database**: Oracle Database 21c Express Edition
- **Key Dependencies**: Spring Data JPA, Hibernate ORM, Oracle JDBC Driver (ojdbc8)

---

## Overview

This migration will transition the Photo Album application from Oracle Database to Azure Database for PostgreSQL. The application currently uses Oracle Database 21c Express Edition to store photo metadata and binary image data as BLOBs. The new architecture will:

- Replace Oracle-specific JDBC drivers and dialects with PostgreSQL equivalents
- Migrate database schema and data from Oracle to Azure PostgreSQL
- Update database connection configuration to use Azure PostgreSQL credentials
- Ensure compatibility of SQL queries and data types between Oracle and PostgreSQL

The migration follows a single-phase approach focusing exclusively on database migration without upgrading Java version, containerization, or deployment to Azure.

---

## Migration Impact Summary

| Application | Original Service | New Azure Service | Authentication | Comments |
|-------------|------------------|-------------------|----------------|----------|
| Photo Album | Oracle DB 21c XE | Azure PostgreSQL  | User/Password  | Migrate DB schema & photo BLOB data |

---

## Code

### Task 1: Migrate from Oracle Database to Azure Database for PostgreSQL

**Description**: Migrate the Photo Album application from Oracle Database to Azure Database for PostgreSQL, including updating dependencies, database dialect, connection configuration, and ensuring data type compatibility.

**Requirements**:
  - Migrate from Oracle Database 21c Express Edition to Azure Database for PostgreSQL
  - Preserve all photo metadata and BLOB data during migration

**Environment Configuration**:
  (To be provided by user if Azure PostgreSQL instance is available)

**Skill**: 
  - Skill Name: migration-oracle-to-postgresql
  - Skill Source: user

**Success Criteria**:
- Pass Build: Yes - Project must compile successfully after migration
- Generate New Unit Tests (Mock-based): No - Skip creating new mock-based unit tests as per user request
- Generate New Integration Tests: No - Skip creating integration tests
- Pass Unit Tests: No - All existing tests must pass
- Pass New Integration Tests: No - No new integration tests to validate
- Security Compliance: No - No CVE validation required

---

## Clarifications

The following items were not explicitly requested but may be needed for a complete implementation:

1. **Azure PostgreSQL Connection Details**
   - **Why needed**: The application needs to connect to an Azure PostgreSQL instance for validation and testing
   - **Options**: 
     - User provides existing Azure PostgreSQL connection details (server, database, username, password)
     - Create a new Azure PostgreSQL instance as part of this migration
     - Test locally with PostgreSQL docker container first, then migrate to Azure later
   - **Recommendation**: If Azure PostgreSQL is not available, test with local PostgreSQL container first using docker-compose

2. **Data Migration Approach**
   - **Why needed**: Existing photos stored as BLOBs in Oracle need to be migrated to PostgreSQL
   - **Options**:
     - Use database export/import tools (pg_dump/restore with intermediary format)
     - Write a custom Java migration utility to read from Oracle and write to PostgreSQL
     - Start with a fresh database (data loss acceptable for development)
   - **Recommendation**: For development/testing, starting fresh is acceptable. For production, a migration script should be created.

3. **Java Version Compatibility**
   - **Why needed**: Java 8 is outdated and may have compatibility issues with modern PostgreSQL drivers
   - **Options**:
     - Keep Java 8 and use older but compatible PostgreSQL JDBC drivers
     - Upgrade to Java 17+ for better compatibility and security (not requested by user)
   - **Recommendation**: Use Java 8 compatible PostgreSQL JDBC driver version as per user's request to avoid Java upgrade
