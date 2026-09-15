# webMethods Employee Integration Platform

A hands-on integration project built with webMethods Integration Server / Service Designer, 
covering core enterprise integration patterns: data transformation, database read/write, 
external API calls, async messaging design, and reliability patterns.

## Overview

This package (`Test1`) implements a set of flow services that read, transform, export, 
and update employee data stored in a SQL Server database (HRDB), plus demonstrates calling 
external REST APIs with timeout/retry handling.

## Services

| Service | Description |
|---|---|
| `getEmployeeId` | Retrieve a single employee by ID, with input validation |
| `GetAllEmployees` | Retrieve all employees, transform each via a reusable service |
| `transformEmployee` | Reusable service that formats a single employee record into a readable string |
| `addEmployee` | Insert a new employee, with department validation, correlation ID, and structured error responses |
| `updateEmployee` | Update an existing employee's details by ID |
| `exportEmployeesAsJSON` / `exportEmployeesAsXML` | Export all employees as JSON or XML |
| `callExternalAPI` | Calls a public REST API with timeout and retry (REPEAT) logic |
| `publishEmployeeAdded` | Publishes an async event when a new employee is added (requires a running Universal Messaging broker) |
| `generateCorrelationId` | Generates a UUID used to trace a request across logs and services |

## Architecture Patterns Demonstrated

- **Adapter Services** — JDBC-based SQL Select/Insert/Update against HRDB
- **Reusable Services** — composition (e.g. `GetAllEmployees` calling `transformEmployee`)
- **Validation** — BRANCH-based input validation with early-exit on failure
- **Structured Errors** — consistent `ErrorResponse` document type across services
- **Correlation IDs** — request tracing threaded through logs and downstream calls
- **Resilience** — timeout + retry (REPEAT) around external API calls
- **Data Transformation** — JSON/XML export via built-in `pub.json` / `pub.xml` services

## Prerequisites

- webMethods Integration Server (developed on version 12.1)
- SQL Server database (`HRDB`) with an `Employee` table
- HRDB JDBC connection alias configured (`HRDBConnection`)

## Known Limitations

- Async publish/subscribe (`publishEmployeeAdded`) requires a local Universal Messaging 
  broker, which isn't set up in this environment — the pattern is implemented but untested live.
- REST API exposure via REST API Descriptor was not completed due to a Designer UI issue; 
  services can still be invoked directly via `http://localhost:5555/invoke/Employees.Data/<serviceName>`.

## Learning Context

Built as part of a 7-day self-directed webMethods integration curriculum covering 
integration fundamentals, Flow Service development, data transformation, database 
integration, messaging/reliability patterns, and API/security concepts.
