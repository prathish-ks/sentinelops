# SentinelOps — Project Charter

## What this is

SentinelOps is a miniature, portfolio-scale platform inspired by the engineering concerns of a digital banking backend: customer records, accounts, and transactions, served by small Go microservices and operated the way a production platform team would operate them — containerized, deployed to Kubernetes on AWS, built and released through CI/CD, secured, observed, and partially self-healing.

**It is not, and must never be described as, a real banking product or a production system.** No real money, customers, or financial data are involved at any point. The "banking" domain is a vehicle for demonstrating platform, security, and reliability engineering — the same practices that apply to any regulated or high-trust backend.

## Problem statement

Job-seeking engineers with strong hands-on skills often struggle to demonstrate *production judgment* — the ability to design, secure, operate, and explain a system end to end — because most portfolio projects stop at "it runs on my laptop." SentinelOps exists to close that gap: a single, coherent project that produces evidence (code, infrastructure, diagrams, runbooks, demos, and rehearsed narratives) of end-to-end platform ownership, built in fixed, reviewable increments.

## Target audience

Hiring managers and interview panels for:

- Platform / infrastructure engineering roles
- Site reliability engineering (SRE) roles
- Security engineering roles
- Engineering leadership roles evaluating breadth of production judgment

The project is optimized to be *walked through in an interview* — architecture, trade-offs, and failure handling explained clearly — not to impress on feature count alone.

## Business scope

A minimal digital-banking-style domain, limited to what is needed to exercise real platform concerns:

- Customers (identity, profile)
- Accounts (ownership, balance state)
- Transactions (transfers/postings between accounts)

No payment rails, card processing, KYC/AML, ledgers of record, or real currency movement are in scope. All data is synthetic.

## Technical scope

- Three Go microservices (customer, account, transaction) with PostgreSQL persistence and REST APIs
- Local developer experience via Docker Compose
- Containerized deployment to Kubernetes (local kind/k3d/minikube, then AWS EKS)
- Infrastructure as code via Terraform (VPC, EKS, RDS, KMS, Secrets Manager, IAM)
- CI/CD via GitHub Actions: test, scan, build, publish, deploy, release
- Practical security engineering: least privilege, encryption, WAF, audit logging, managed threat detection, an intentionally insecure lab with a detector and remediation examples
- Observability and reliability: metrics, logs, traces, SLIs/SLOs, alerting, runbooks, failure injection, and bounded automated recovery
- A governed AI operations layer: an evidence-grounded incident analyst with approval-gated, auditable remediation suggestions — never autonomous, unreviewed changes

## Explicit non-goals

- Not a production or commercial banking system, and never represented as one
- No real customer, financial, or payment data — synthetic data only, and nothing that resembles real account or card numbers
- No multi-tenant SaaS concerns (billing, tenant isolation at scale, compliance certification)
- No mobile or web frontend beyond what's needed to demonstrate the APIs
- No high-availability multi-region deployment — single-region is sufficient to demonstrate the pattern
- No unbounded feature growth — CRTO (offensive security) work is a parallel learning track, not a dependency of SentinelOps v1.0
- Not an exhaustive implementation of any single practice (e.g., full compliance-grade audit logging) — depth is demonstrated, not exhaustively production-hardened

## Architectural principles

1. **Boring technology, clearly documented.** Prefer well-understood, idiomatic choices (Go, PostgreSQL, Kubernetes, Terraform) over novelty, so the design decisions — not the tooling — are the interview story.
2. **Explicit boundaries.** Every service, trust boundary, and external dependency is drawn and named in the architecture docs before it's built.
3. **Everything as code.** Infrastructure, deployment, and policy are versioned and reproducible, not manually configured.
4. **Small, reversible increments.** Each unit of work produces a validated, committed artifact rather than a large, unreviewable change.

## Security principles

1. **Least privilege by default** — IAM, RBAC, and network policy default-deny and are opened up only where a component demonstrably needs access.
2. **No real secrets, ever.** Placeholders, environment variables, workload identity/OIDC, and secret-management patterns only; insecure examples live in an isolated, disabled-by-default lab.
3. **Defense in depth, demonstrated deliberately.** Encryption, network segmentation, WAF, audit logging, and detection are each implemented as a visible, explainable control — not assumed.
4. **Detect and remediate, not just prevent.** The project includes an intentionally insecure scenario, a detector, and remediation, because interviews reward the ability to respond to a live problem as much as the ability to prevent one.

## Definition of done (v1.0.0)

SentinelOps v1.0 is complete when all of the following exist and are true:

- All nine phases (0–8) are tagged as milestones with passing validation for every task
- A public GitHub repository with a hiring-manager-scannable README, architecture diagrams, and an evidence index
- Three recorded/scripted demos: deployment & security, break-and-recover (reliability), and AI-assisted diagnosis
- A 10-story interview map, with every story answerable aloud without notes
- A public-release safety review confirming no real credentials, tokens, account IDs, or proprietary/customer data are present anywhere in the repository or its history

## Hard rule

**Prefer demonstrable depth over feature breadth.** When a choice arises between adding a new capability and making an existing capability more clearly engineered, documented, and defensible in an interview, choose depth. New ideas that don't materially strengthen a listed outcome above go to the backlog, not the critical path.
