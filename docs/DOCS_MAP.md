# DOCUMENTATION LOCATION MAP - PRECISE RULES FOR AI AGENTS

## EXACT DIRECTORY PATHS AND CONTENT PLACEMENT RULES

| File Type | EXACT Directory Path | No Exceptions |
|-----------|---------------------|---------------|
| **CHANGELOG FILES** | `/docs/changelogs/` | ALL implemented changes. Format: YYYY-MM-DD-name.md |
| **ISSUE DOCUMENTATION** | `/docs/issues/` | ALL bug reports, problems, investigations |
| **API DOCUMENTATION** | `/docs/documentation/api/` | API guides, error codes, endpoints ONLY |
| **AUTH DOCUMENTATION** | `/docs/documentation/auth/` | Authentication, login flows, tokens ONLY |
| **DATABASE DOCUMENTATION** | `/docs/documentation/db/` | Database schemas and queries ONLY |
| **EMAIL DOCUMENTATION** | `/docs/documentation/email/` | Email templates and systems ONLY |
| **IMPLEMENTATION & PROCESS DOCUMENTATION** | `/docs/documentation/process/` | Code patterns, standards, and documentation processes |
| **INFRASTRUCTURE DOCUMENTATION** | `/docs/documentation/infrastructure/` | Hosting, networking, security ONLY |
| **LOGGING DOCUMENTATION** | `/docs/documentation/logging/` | Logging formats and monitoring ONLY |
| **STRIPE DOCUMENTATION** | `/docs/documentation/stripe/` | Stripe integration, webhooks, payments ONLY |
| **USER DOCUMENTATION** | `/docs/documentation/users/` | User management ONLY |
| **IXC DOCUMENTATION** | `/docs/ixc/` | ALL IXCoach-specific documentation |
| **IXC ISSUES** | `/docs/ixc/issues/` | ALL IXCoach-specific issues |
| **TEST DOCUMENTATION** | `/docs/tests/` | ALL test plans and reports |
| **INITIATIVE DOCUMENTATION** | `/docs/initiatives/` | ALL strategic initiative plans |
| **INTEGRATION DOCUMENTATION** | Various specific integration folders | ALL external service integrations (hubspot, slack, etc) |
| **CHANGE REVIEW** | `/docs/deprecated/change-review/` | ALL change review process documentation |
| **SLACK DOCUMENTATION** | `/docs/slack/` | ALL Slack integration documentation |
| **SECURITY DOCUMENTATION** | `/docs/security/` | ALL security audit documentation |
| **SUBSCRIPTION DOCUMENTATION** | `/docs/subscription/` | ALL subscription management documentation |
| **COOKIES DOCUMENTATION** | `/docs/cookies/` | ALL cookie-related documentation |
| **PROPOSED CHANGES** | `/docs/issues/proposed/` | ALL proposed but not implemented changes |
| **DEPRECATED CONTENT** | `/docs/deprecated/` | ALL deprecated documentation |
| **COMPLETED CONTENT** | `/docs/issues/done/` | ALL completed work documentation |

## NESTED DIRECTORY STRUCTURE - COMPLETE HIERARCHY

```
/docs/
├── DOCS_MAP.md           # THIS MAP FILE ONLY
├── changelogs/           # CHANGELOG FILES ONLY - Format: YYYY-MM-DD-name.md
├── cookies/              # COOKIES DOCUMENTATION ONLY
├── deprecated/           # DEPRECATED DOCUMENTATION ONLY
│   ├── change-review/    # CHANGE REVIEW DOCUMENTATION ONLY
│   └── change_review/    # VARIANT DIRECTORY - LEGACY
├── documentation/        # REFERENCE DOCUMENTATION ONLY - Domain-specific subfolders
│   ├── api/              # API DOCUMENTATION ONLY
│   ├── auth/             # AUTH DOCUMENTATION ONLY
│   ├── authentication/   # AUTH DOCUMENTATION VARIANT
│   ├── db/               # DATABASE DOCUMENTATION ONLY
│   ├── email/            # EMAIL DOCUMENTATION ONLY
│   ├── infrastructure/   # INFRASTRUCTURE DOCUMENTATION ONLY
│   ├── issues/           # DOCUMENTATION ISSUES ONLY
│   ├── loggers/          # LOGGING DOCUMENTATION VARIANT
│   ├── logging/          # LOGGING DOCUMENTATION ONLY
│   ├── notifications/    # NOTIFICATION DOCUMENTATION ONLY
│   ├── payment_flow/     # PAYMENT FLOW DOCUMENTATION ONLY
│   ├── process/          # IMPLEMENTATION & PROCESS DOCUMENTATION 
│   ├── stripe/           # STRIPE DOCUMENTATION ONLY
│   ├── subscriptions/    # SUBSCRIPTION DOCUMENTATION VARIANT
│   ├── testing/          # TEST DOCUMENTATION VARIANT
│   └── users/            # USER DOCUMENTATION ONLY
├── frontend/             # FRONTEND DOCUMENTATION ONLY
├── hubspot/              # HUBSPOT INTEGRATION DOCUMENTATION ONLY
├── initiatives/          # INITIATIVE DOCUMENTATION ONLY
├── issues/               # ISSUE DOCUMENTATION ONLY
│   ├── GoogleAuthDetectionIssue/   # GOOGLE AUTH ISSUES ONLY
│   ├── Sign_in_bugs_Issue_Apr_2_2025/  # SIGN-IN BUGS ONLY
│   ├── api_test_issues/  # API TEST ISSUES ONLY
│   ├── done/             # COMPLETED WORK DOCUMENTATION ONLY
│   ├── login/            # LOGIN ISSUES ONLY
│   ├── payment-authentication-flow/ # PAYMENT AUTH ISSUES ONLY
│   ├── proposed/         # PROPOSED CHANGES ONLY
│   ├── test_failures/    # TEST FAILURE DOCUMENTATION ONLY
│   └── webhook-issues/   # WEBHOOK ISSUES ONLY
├── ixc/                  # IXC DOCUMENTATION ONLY
│   └── issues/           # IXC ISSUES ONLY
│       ├── circular_dependencies/  # DEPENDENCY ISSUES ONLY
│       ├── firebase_auth_issues/   # FIREBASE AUTH ISSUES ONLY
│       └── inconsistent_props/     # PROP CONSISTENCY ISSUES ONLY
├── learnings/            # LEARNING DOCUMENTATION ONLY
├── security/             # SECURITY DOCUMENTATION ONLY
├── slack/                # SLACK DOCUMENTATION ONLY
├── subscription/         # SUBSCRIPTION DOCUMENTATION ONLY
│   └── phases/           # SUBSCRIPTION IMPLEMENTATION PHASES ONLY
└── tests/                # TEST DOCUMENTATION ONLY
    ├── reports/          # TEST REPORTS ONLY
    └── webhook-failures/ # WEBHOOK TEST FAILURES ONLY
```

## MANDATORY RULES FOR DOCUMENT PLACEMENT

1. ALL changelog files MUST be placed in `/docs/changelogs/` with YYYY-MM-DD-name.md format

2. ALL issue files MUST be placed in `/docs/issues/` or the appropriate issue subfolder

3. ALL reference documentation MUST be placed in the appropriate domain subfolder:
   - API docs → `/docs/documentation/api/`
   - Auth docs → `/docs/documentation/auth/`
   - Email docs → `/docs/documentation/email/`
   - etc.

4. NEVER create new top-level folders without explicit human approval

5. NEVER place documentation in the root `/docs/` directory - use the appropriate subfolder

6. ALWAYS use existing folders - DO NOT create new domain folders without human approval

7. REFERENCE this map when determining where to place new documentation

8. DO NOT place documentation in a folder when its topic doesn't match the folder's exact purpose

9. HONOR EXISTING FOLDER STRUCTURE - if a folder exists, use it for its intended purpose

## DECISION FLOWCHART FOR DOCUMENT PLACEMENT

1. Is the document about a completed change? → `/docs/changelog/`
2. Is the document about an issue or bug? → `/docs/issues/` or appropriate subfolder
3. Is the document IXCoach-specific? → `/docs/ixc/` or `/docs/ixc/issues/`
4. Is the document about tests? → `/docs/tests/`
5. Is the document about a strategic initiative? → `/docs/initiatives/`
6. Is the document about an external integration? → Use appropriate integration folder (`/docs/hubspot/`, `/docs/slack/`, etc.)
7. Is the document about change review process? → `/docs/deprecated/change-review/`
8. Is the document about Slack integration? → `/docs/slack/`
9. Is the document about security? → `/docs/security/`
10. Is the document about subscription management? → `/docs/subscription/`
11. Is the document about cookies? → `/docs/cookies/`
12. Is the document about proposed changes? → `/docs/issues/proposed/`
13. Is the document deprecated? → `/docs/deprecated/`
14. Is the document about completed work? → `/docs/issues/done/`
15. Otherwise, determine the exact domain and place in `/docs/documentation/{domain}/`