# Security Policy

## Supported Versions

We actively maintain and patch the latest release of each cashflytic service.

| Version | Supported |
|---------|----------|
| Latest  | ✅        |
| Older   | ❌        |

## Reporting a Vulnerability

**Please do not open a public GitHub issue for security vulnerabilities.**

Cashflytic handles sensitive financial data, and responsible disclosure is critical. If you discover a vulnerability, please report it privately:

**Email:** security@cashflytic.com

Include the following in your report:
- A description of the vulnerability and its potential impact
- Steps to reproduce (proof-of-concept if possible)
- Affected component(s) and version(s)
- Any suggested remediation

## Response SLA

| Milestone | Target |
|-----------|--------|
| Acknowledgement | Within 48 hours |
| Initial assessment | Within 5 business days |
| Patch for critical/high severity | Within 14 days |
| Patch for medium severity | Within 30 days |
| Patch for low severity | Within 90 days |

We will keep you informed throughout the process and credit you in the release notes unless you prefer to remain anonymous.

## Scope

The following are **in scope** for vulnerability reports:

- **Financial data handling**: expense tracking, transaction processing, budget calculations
- **Authentication & authorisation**: login flows, session management, token handling, role-based access
- **Receipt OCR pipeline**: Mistral API integration, file upload handling, data extraction
- **Uber integration**: OAuth flow, API credential handling, trip data processing
- **API endpoints**: injection attacks, broken access control, sensitive data exposure
- **Data storage**: PostgreSQL queries, data-at-rest exposure

## Out of Scope

The following are **not** eligible for reports:

- Rate limiting and brute-force protections on non-sensitive endpoints
- Denial-of-service (DoS/DDoS) attacks
- Vulnerabilities in third-party services (report directly to the vendor)
- Missing security headers on non-sensitive static resources
- Self-XSS or issues requiring physical access to a logged-in device
- Social engineering of cashflytic employees
- Theoretical vulnerabilities without a working proof of concept

## Disclosure Policy

We follow a coordinated disclosure model. We ask that you:

1. Give us reasonable time to investigate and patch before public disclosure
2. Avoid accessing, modifying, or deleting data belonging to other users
3. Do not perform actions that could disrupt service availability

We will not take legal action against researchers who follow these guidelines in good faith.
