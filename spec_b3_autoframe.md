# AutoFrame — Automation Scaffolding & Boilerplate Generator

## Product Specification (High-Level) — v0.1

**Category:** Internal Delivery Accelerator (Set B) — dual-purpose: internal-first, client-facing later
**Accelerates:** Track B Product 3 (Process Automation Build), Track B Product 4 (Custom AI-Powered Tool)
**Owner:** Lukas
**Status:** Specification Draft

---

## Elevator Pitch

AutoFrame is a project scaffolding tool that generates ready-to-customize automation codebases from structured pattern definitions. Instead of starting every automation project from a blank file, AutoFrame produces a complete project skeleton — with configuration management, error handling, logging, input/output contracts, and documentation — in seconds. The consultant then fills in only the business-specific logic.

Every automation project shares ~60–70% of its infrastructure code: reading configuration, handling errors, logging execution, managing secrets, validating inputs, formatting outputs. AutoFrame codifies these patterns into reusable, composable templates. The tool doesn't write the business logic — it eliminates everything around it, so the consultant can focus on the part that actually differs between projects.

---

## Value Realization

### For Internal Use (Primary)

- **Time savings:** Eliminate 2–4 hours of boilerplate setup per automation project. For a Track B Basic-tier build (3–5h total), this reclaims 40–60% of the delivery time for actual business logic.
- **Quality baseline:** Every generated project includes production-grade error handling, logging, and configuration from the start — not as an afterthought bolted on at the end.
- **Consistency:** All delivered automations share the same structure, making them easier to maintain, hand over, and extend. Clients who order multiple automations get a coherent codebase, not a collection of one-off scripts.
- **Speed to first demo:** Within 30 minutes of starting a project, the consultant has a running skeleton that can be shown to the client as a "structure preview" — building confidence before the real work begins.
- **LLM development synergy:** Clean, well-structured scaffolds with clear contracts and docstrings make LLM-assisted code generation significantly more effective. The LLM writes better business logic when the surrounding infrastructure is already solid.

### For Client-Facing Use (Future)

- **Transparency:** Client can see the project structure and quality standards before business logic is added. Builds trust in the deliverable.
- **Self-service starter kits:** Clients who want to build their own automations can use AutoFrame patterns as starting points, with the consultant providing the template selection and configuration.
- **Maintenance handover:** When a client's internal team takes over a delivered automation, the standardized structure makes onboarding dramatically easier.

---

## Prerequisites for Client Use

### What the Client Must Provide

- **Automation brief** describing:
  - What process to automate (inputs, steps, outputs)
  - What systems are involved (data sources, APIs, file systems, databases)
  - Expected trigger mechanism (scheduled, event-driven, manual)
  - Output format and destination (file, database, API call, email, dashboard)
  - Error handling expectations (retry? alert? log and continue?)
- **Access credentials** for involved systems (API keys, database connections, file shares) — provided securely, never stored in scaffolded code

### Consultant Prerequisites

- Python 3.10+ runtime environment
- Cookiecutter or similar templating engine
- LLM API access (for generating business logic stubs and documentation)

---

## What Can Specifically Be Automated?

### Fully Automatable (Template-Based)

| Function | What the Tool Does | Output |
|----------|-------------------|--------|
| **Project structure generation** | Create a complete project directory with standardized layout: src/, config/, tests/, docs/, scripts/ | Ready-to-use project directory |
| **Configuration management** | Generate config file structure with environment-based overrides (dev/staging/prod), secret placeholders, validation | Config module + .env template + validation code |
| **Error handling framework** | Generate try/except structures with categorized error types, retry logic, graceful degradation patterns | Error handling module with custom exception classes |
| **Logging setup** | Configure structured logging with rotation, level control, and output formatting (console + file) | Logging module, pre-configured for the project |
| **Input/output contracts** | Generate data models (Pydantic or dataclasses) for expected inputs and outputs based on the automation brief | Data model module with validation |
| **Testing scaffold** | Generate test file structure with fixtures, sample data, and test function stubs for each automation step | Test directory with pytest setup |
| **Documentation skeleton** | Generate README, setup instructions, architecture description, and usage guide templates | docs/ directory with markdown files |
| **Deployment artifacts** | Generate Dockerfile, requirements.txt, Makefile, and basic CI/CD config | Deployment-ready project files |
| **Common integration stubs** | Generate connection boilerplate for common integration targets: REST APIs, databases, file systems, email, cloud storage | Integration module with connection management |

### Semi-Automatable (Pattern Matching + LLM-Assisted)

| Function | What the Tool Does | Output |
|----------|-------------------|--------|
| **Pattern selection** | Analyze the automation brief and recommend the best combination of patterns (ETL, event-driven, scheduled batch, API gateway, etc.) | Recommended pattern set with rationale |
| **Business logic stubs** | Generate function stubs with docstrings, type hints, and pseudocode comments describing what each function should do | Stub functions ready for implementation |
| **Data transformation templates** | Based on described input/output formats, generate transformation function templates with sample data | Transformation module with examples |
| **Orchestration logic** | Generate the main execution flow connecting all steps in the right order, with conditional branching where described | Main orchestrator module |

### Not Automatable (Consultant Value-Add)

- Actual business logic implementation (the core "what does this automation do" code)
- Integration testing with real client systems and data
- Performance tuning for specific data volumes and system constraints
- Edge case identification and handling beyond what the brief describes
- Deployment to client's specific infrastructure

---

## MVP / Proof of Concept

### Scope

The MVP focuses on **pattern-based project generation for the three most common automation types**, using a template engine with composable modules. It should be usable for the first Track B "Process Automation Build" gig.

### MVP Feature Set

1. **Pattern Library (3 Core Patterns)**

   **Pattern A: Data Pipeline (ETL/ELT)**
   - Read from source (file, API, database)
   - Transform data (clean, map, validate, enrich)
   - Load to destination (file, database, API)
   - Scheduling support (cron-compatible)

   **Pattern B: API Integration Automation**
   - Connect to external API(s) with authentication
   - Fetch/send data on schedule or trigger
   - Transform between API formats
   - Error handling with retry and alerting

   **Pattern C: Document/File Processing**
   - Watch or read input files (CSV, Excel, PDF, JSON)
   - Extract, transform, or classify content
   - Generate output files or reports
   - File management (archiving, naming conventions)

2. **Project Generator CLI**
   - Interactive questionnaire: project name, pattern selection, integration targets, trigger type, output format
   - Generate complete project from selected pattern + options
   - Composable: combine elements from multiple patterns if needed

3. **Generated Project Contents**
   - Standard directory structure
   - Configuration with .env template and validation
   - Error handling framework with custom exceptions
   - Structured logging (console + file)
   - Input/output data models (Pydantic)
   - Main orchestrator with step-by-step execution flow
   - Integration stubs for selected targets (2–3 common ones: REST API, PostgreSQL, file system)
   - Test scaffolding (pytest) with fixtures and sample data
   - README with setup and usage instructions
   - requirements.txt and basic Dockerfile

4. **Customization Layer**
   - All generated code includes clear `# TODO: Implement business logic here` markers
   - Each module has docstrings explaining purpose, expected behavior, and extension points
   - Configuration is externalized — changing behavior doesn't require code changes where possible

### MVP Technical Architecture

```
Input: Interactive CLI Questionnaire
  → Pattern Selector (map answers to pattern combination)
    → Template Engine (Cookiecutter / Jinja2)
      → Module Composer (select and combine template modules)
        → Code Generator (render templates with project-specific variables)
          → Post-Generation Hook (install dependencies, initialize git, validate structure)
            → Complete Project Directory
```

### MVP Effort Estimate

| Component | Estimated Effort | Notes |
|-----------|-----------------|-------|
| Pattern A template (Data Pipeline) | 4–6h | Most common, build first |
| Pattern B template (API Integration) | 3–5h | Reuses infrastructure from Pattern A |
| Pattern C template (Document Processing) | 3–5h | Reuses infrastructure from Pattern A |
| Shared infrastructure modules (config, logging, errors, models) | 4–5h | Reused across all patterns |
| CLI interface + pattern selector | 2–3h | Interactive questionnaire |
| Template engine integration (Cookiecutter) | 2–3h | Wiring templates to the generator |
| Testing + validation with 2 realistic projects | 2–3h | Generate and verify end-to-end |
| **Total MVP** | **20–30h** | ~3–4 weekends of focused work |

### MVP Success Criteria

- Generate a complete, runnable project skeleton for each of the 3 patterns in under 60 seconds
- Generated code passes linting (flake8/ruff) and basic structural tests out of the box
- Reduce setup time for a new automation project from 2–4h to under 15 minutes
- Consultant needs to write only the business logic — all infrastructure is provided
- Generated project is clean enough that a client's developer could pick it up and understand the structure without guidance

---

## Perspective Features (v2 / v3)

### v2: More Patterns & Intelligence

- **Additional patterns:** Scheduled reporting, webhook handler, multi-system sync, AI/LLM-powered processing pipeline, event-driven (message queue consumer/producer)
- **Stack variants:** Generate the same pattern in different tech stacks (Python + Airflow, Python + Prefect, Node.js, or using low-code platforms like n8n or Make)
- **LLM-assisted business logic generation:** After scaffolding, feed the automation brief into an LLM along with the generated structure. The LLM writes first-draft implementations for each business logic stub. Consultant reviews and refines.
- **Integration library expansion:** Pre-built connection modules for 15–20 common services (Slack, email/SMTP, S3, Google Sheets, Salesforce, HubSpot, etc.)
- **Configuration UI:** Web-based questionnaire instead of CLI, with visual preview of the generated project structure before generation.

### v3: Client-Facing Product

- **Automation starter kits:** Packaged project templates that clients can purchase and customize themselves. Each kit includes the scaffold, documentation, and a video walkthrough.
- **Custom scaffold service:** Client describes their automation need, receives a fully scaffolded project with stubs, documentation, and architecture diagram. They (or their developer) fill in the business logic. Positioned as a "blueprint" tier below the full build.
- **White-label:** Configurable branding on generated documentation and code comments
- **Project estimation engine:** Based on pattern selection and complexity indicators, auto-generate effort estimates and cost projections. Useful for Track A proposals and Track B gig scoping.
- **Template marketplace:** Allow other consultants/developers to contribute and use patterns. Community-driven pattern library (long-term).
- **CI/CD integration:** Generated projects include ready-to-use GitHub Actions / GitLab CI configurations matched to the project's deployment target

---

*Document version: 0.1 — March 2026*
*Next step: MVP build (Phase 2, WS-2.4 — after SchemaLens and SpecForge MVPs)*
