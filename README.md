# DevPartner AI

### AI-Powered Developer Workflow for Safe, Verified Code Changes

DevPartner AI is an AI-assisted developer workflow designed to help developers understand, plan, verify, and evaluate code changes before accepting them.

Rather than treating AI-generated code as something that should be trusted immediately, DevPartner treats every proposed modification as a **hypothesis that must be analyzed, reviewed, and challenged**.

The workflow connects developer intent, invariant identification, dependency and impact analysis, risk assessment, implementation planning, human approval, isolated patching, verification, mutation testing, adversarial counterexamples, and evidence generation.

> **Core idea:** Generate less blindly. Verify more intelligently.

---

## Table of Contents

- [Overview](#overview)
- [The Problem](#the-problem)
- [Our Solution](#our-solution)
- [How DevPartner Works](#how-devpartner-works)
- [Core Features](#core-features)
- [What Makes DevPartner Different?](#what-makes-devpartner-different)
- [Technical Architecture](#technical-architecture)
- [Project Structure](#project-structure)
- [Important Implementation Files](#important-implementation-files)
- [Technology Stack](#technology-stack)
- [Getting Started](#getting-started)
- [Environment Variables](#environment-variables)
- [Demonstration](#demonstration)
- [Project Evidence and Documentation](#project-evidence-and-documentation)
- [IBM Bob Assisted Development](#ibm-bob-assisted-development)
- [Vision](#vision)

---

## Overview

AI coding assistants make software development faster by helping developers generate code, understand existing implementations, and solve programming problems. However, generating a code change is only one part of the development process. Developers must also understand its consequences and ensure that existing behavior remains correct.

DevPartner AI addresses this challenge through a structured **verify-before-you-merge workflow**.

A developer starts by describing a requested change in natural language. DevPartner processes that request and guides it through a sequence of analysis, planning, approval, implementation, and verification stages.

The system is designed to help answer important questions:

- What behavior is the developer actually requesting?
- Which rules and requirements must remain true?
- Which files or components could be affected?
- What risks could the proposed change introduce?
- What should be changed, and how should it be tested?
- Does the resulting implementation satisfy the expected behavior?
- Can a counterexample expose a weakness in the implementation or its tests?
- What evidence supports the final decision?

The result is a more transparent and controlled approach to AI-assisted development.

---

## The Problem

AI coding assistants can generate code quickly, but generated changes may introduce problems that are not immediately visible.

Even a small modification can:

- Break existing functionality.
- Affect unexpected parts of a codebase.
- Violate security requirements or business rules.
- Introduce regressions into previously working features.
- Pass basic tests while failing on edge cases.
- Create unexpected dependencies between components.
- Make it difficult to understand the actual impact of a change.

This creates an important gap between **code generation and code correctness**.

Developers still need to spend time understanding the codebase, reviewing modifications, analyzing dependencies, validating behavior, and investigating failures.

Simply generating code does not guarantee that the resulting software is safe or correct.

### The Need

A reliable AI-assisted development process should do more than generate an implementation. It should also:

1. Understand the requested behavior.
2. Identify the conditions that must remain true.
3. Analyze the potential impact of a modification.
4. Assess risks before implementation.
5. Allow developers to review the proposed plan.
6. Verify the resulting behavior.
7. Challenge the implementation with possible failure scenarios.
8. Present evidence that helps developers make an informed decision.

---

## Our Solution

DevPartner AI introduces a structured workflow that places analysis and verification around AI-assisted code changes.

Instead of immediately applying a generated modification, the workflow first builds an understanding of the request and its potential consequences.

The developer can review the implementation plan before proceeding. After an approved change is applied through the isolated patch workflow, the system performs verification and adversarial testing.

The workflow concludes with an evidence report that brings the relevant analysis and results together.

### End-to-End Workflow

```text
              Developer Request
                     |
                     v
                Intent Extraction
                     |
                     v
             Invariant Identification
                     |
                     v
                Impact Analysis
                     |
                     v
                Risk Assessment
                     |
                     v
              Implementation Plan
                     |
                     v
               Human Approval
                     |
                     v
              Isolated Patch
                     |
                     v
                 Verification
                     |
                     v
               Mutation Testing
                     |
                     v
          Adversarial Counterexamples
                     |
                     v
                Evidence Report
                     |
                     v
              Accept / Rollback
```

This process makes the proposed change easier to understand, inspect, and evaluate before it becomes part of the final codebase.

---

## How DevPartner Works

The workflow can be understood through four major phases.

### Phase 1: Understand and Analyze

The developer describes the required change. DevPartner extracts the intended behavior, identifies important invariants, and analyzes the relationships between affected project components.

**Output:** A structured understanding of the request, requirements, affected areas, and potential risks.

### Phase 2: Plan and Approve

Using the analysis, DevPartner prepares an implementation plan. The plan describes the files that may need modification, the expected changes, relevant tests, and potential risks.

The developer reviews the plan before the modification proceeds.

**Output:** A reviewed implementation plan with a human approval checkpoint.

### Phase 3: Implement and Verify

After approval, the change proceeds through the isolated patch workflow. DevPartner then checks the resulting behavior against the expected requirements.

**Output:** A proposed code change and its verification results.

### Phase 4: Challenge and Generate Evidence

The system challenges the strength of the verification process through mutation testing and adversarial counterexample analysis. Relevant results are consolidated into an evidence report.

**Output:** Verification information, identified weaknesses or counterexamples, and supporting evidence for the final decision.

---

## Core Features

### 1. Intent Extraction

DevPartner analyzes a developer's natural-language request to understand the intended behavior and requirements.

Instead of treating a request as an unstructured instruction, this stage creates a structured understanding of what the developer wants to change.

**Why it matters:**

- Clarifies the expected behavior.
- Helps identify the scope of a requested modification.
- Provides a foundation for subsequent analysis and planning.

### 2. Invariant Identification

An invariant is a condition or rule that must remain true even after a code modification.

DevPartner identifies important behavioral and security requirements and represents them as explicit invariants.

**Examples:**

- Unauthorized users must not access protected resources.
- Existing API contracts must remain valid.
- Required input validation must continue to work.
- Important business rules must not be violated.

**Why it matters:**

Invariants provide explicit conditions against which a proposed change can be evaluated. They help shift the focus from simply checking whether code executes to checking whether expected rules remain valid.

**Related implementation:**
- `src/components/InvariantPanel.tsx`

### 3. Impact Analysis

A change in one part of an application may affect other components through dependencies and relationships.

DevPartner analyzes project component relationships to identify areas that could be affected by a proposed modification.

The impact analysis helps developers understand the potential scope of a change before implementation.

**Why it matters:**

- Highlights potentially affected components.
- Makes dependencies easier to understand.
- Helps developers identify areas that may need additional testing.

**Related implementation:**
- `src/lib/graph.ts`
- `src/components/ImpactMap.tsx`

### 4. Risk Assessment

Not every code modification carries the same level of risk.

DevPartner evaluates potential risks associated with the proposed change, considering affected areas and identified requirements.

The risk assessment helps developers understand which parts of the modification may need closer attention during implementation and verification.

**Why it matters:**

- Draws attention to potentially sensitive changes.
- Helps prioritize verification.
- Provides additional context before implementation.

### 5. AI-Assisted Implementation Planning

Before modifying the project, DevPartner generates an implementation plan.

The plan can identify:

- Files that may need modification.
- Expected changes in each relevant area.
- Tests that should be performed.
- Potential implementation risks.
- Rollback considerations.

**Why it matters:**

Planning provides developers with visibility into the proposed approach before changes are applied.

### 6. Human Approval Gate

DevPartner includes a human approval stage before the modification proceeds.

The developer can review the proposed plan and decide whether the workflow should continue.

This keeps the developer involved in important implementation decisions instead of blindly accepting every AI-generated suggestion.

**Why it matters:**

- Keeps the developer in control.
- Encourages review before implementation.
- Makes the workflow more transparent.

### 7. Isolated Patch Application

Approved changes can be applied through an isolated patch workflow instead of immediately modifying the final codebase.

This approach provides a safer environment for evaluating proposed modifications.

**Why it matters:**

- Separates proposed changes from immediate final acceptance.
- Supports controlled experimentation.
- Makes rollback considerations part of the workflow.

### 8. Verification

After a change is applied, DevPartner performs verification against the expected behavior and requirements.

This stage is intended to determine whether the modification satisfies the conditions established earlier in the workflow.

**Why it matters:**

- Evaluates the proposed implementation.
- Checks behavior against expected requirements.
- Provides verification information for the evidence report.

**Related implementation:**
- `src/lib/harness.ts`
- `src/components/VerificationPanel.tsx`

### 9. Mutation Testing

Mutation testing challenges the strength of a verification process by introducing controlled changes, known as mutations, and checking whether the tests or verification process detect them.

For example, a mutation may deliberately alter a condition or expected behavior. If the verification process does not detect that alteration, it may indicate a weakness in the tests.

**Why it matters:**

- Helps evaluate the effectiveness of tests.
- Identifies cases that may not be adequately covered.
- Provides a stronger challenge than relying only on successful test execution.

### 10. Adversarial Counterexamples

One of DevPartner's key ideas is to actively search for scenarios in which a proposed change could violate an invariant.

Instead of only asking:

> Does the code work?

DevPartner also asks:

> Can we find a situation where the expected rule stops being true?

Potential examples include:

- Forged user roles.
- Missing authorization checks.
- Invalid input.
- Boundary conditions.
- Unexpected dependency behavior.

A counterexample helps illustrate a specific situation in which the expected behavior may fail.

**Why it matters:**

- Encourages testing beyond normal execution paths.
- Helps expose potential weaknesses.
- Connects verification results to explicit requirements.

**Related implementation:**
- `src/components/CounterexamplePanel.tsx`

### 11. Evidence Report

DevPartner consolidates relevant information from the workflow into an evidence view.

The report can include:

- Original developer intent.
- Identified invariants.
- Impact analysis.
- Risk information.
- Proposed implementation changes.
- Verification results.
- Mutation testing results.
- Adversarial counterexamples.
- Supporting artifacts.

**Why it matters:**

The evidence report gives developers a consolidated view of the analysis and verification process. It makes the reasoning and results behind a proposed change easier to inspect.

**Related implementation:**
- `src/components/EvidencePanel.tsx`

---

## What Makes DevPartner Different?

A conventional AI coding workflow often follows a simple process:

```text
Developer Request
       |
       v
AI Generates Code
       |
       v
Developer Reviews Code
```

DevPartner extends this approach by introducing analysis, explicit requirements, human approval, verification, and adversarial testing.

```text
Developer Request
       |
       v
Understand Intent
       |
       v
Identify Invariants
       |
       v
Analyze Impact
       |
       v
Assess Risk
       |
       v
Generate Implementation Plan
       |
       v
Human Approval
       |
       v
Apply Isolated Change
       |
       v
Verify Expected Behavior
       |
       v
Challenge with Counterexamples
       |
       v
Generate Evidence
       |
       v
Accept or Rollback
```

| Conventional AI Coding | DevPartner AI |
|---|---|
| Focuses primarily on code generation | Focuses on the complete change workflow |
| Relies heavily on developer review after generation | Introduces analysis and planning before implementation |
| May not explicitly define expected behavior | Identifies invariants and requirements |
| Provides limited visibility into change impact | Includes dependency and impact analysis |
| Typically checks whether the code works | Also challenges verification through mutation testing and counterexamples |
| May leave evidence scattered across the workflow | Consolidates relevant information into an evidence report |

**The objective is not simply to make AI coding faster.**

The objective is to make AI-assisted software changes more explainable, testable, auditable, and controllable.

---

## Technical Architecture

DevPartner AI is organized into application components, workflow orchestration, AI-assisted processing, graph-based impact analysis, and verification functionality.

### Main Architectural Components

| Component | Responsibility |
|---|---|
| Frontend application | Provides the developer-facing interface |
| Workflow orchestration | Connects and manages the stages of the DevPartner workflow |
| AI-assisted processing | Supports intent analysis and implementation planning |
| Graph and dependency analysis | Represents component relationships and supports impact analysis |
| Verification harness | Supports verification and testing workflows |
| UI result panels | Display invariants, impact, verification, counterexamples, and evidence |

### Workflow Orchestration

The central workflow is organized through:

`src/hooks/usePipeline.ts`

This hook coordinates the different stages of the DevPartner workflow, connecting request processing, analysis, planning, verification, and evidence generation.

### AI and Analysis Layer

The AI-assisted processing functionality is organized through:

`src/lib/ai.ts`

The graph analysis functionality is organized through:

`src/lib/graph.ts`

Together, these modules support developer intent processing, planning, dependency analysis, and understanding the potential impact of proposed changes.

### Verification Layer

The verification workflow is implemented through:

`src/lib/harness.ts`

It supports the verification and adversarial-testing functionality used in the application.

### Presentation Layer

The React components provide dedicated views for the different workflow results, including:

- Impact visualization.
- Invariant information.
- Verification results.
- Counterexamples.
- Consolidated evidence.

---

## Project Structure

The following structure represents the main project files and directories documented for DevPartner AI.

```text
Dev-Partner-Antigravity/
│
├── src/
│   │
│   ├── components/
│   │   ├── ImpactMap.tsx
│   │   ├── InvariantPanel.tsx
│   │   ├── CounterexamplePanel.tsx
│   │   ├── VerificationPanel.tsx
│   │   └── EvidencePanel.tsx
│   │
│   ├── demo/
│   │
│   ├── hooks/
│   │   └── usePipeline.ts
│   │
│   ├── lib/
│   │   ├── ai.ts
│   │   ├── harness.ts
│   │   └── graph.ts
│   │
│   ├── App.tsx
│   ├── index.css
│   └── main.tsx
│
├── public/
│
├── server/
│
├── docs/
│   ├── implementation-evidence/
│   └── ibm-bob-evidence/
│
├── package.json
├── index.html
├── tsconfig.json
├── vite.config.ts
└── README.md
```

---

## Important Implementation Files

The following table explains the responsibilities of the main source files.

| File | Purpose |
|---|---|
| `src/hooks/usePipeline.ts` | Main DevPartner workflow orchestration |
| `src/lib/ai.ts` | AI-assisted intent processing and planning |
| `src/lib/harness.ts` | Verification and testing workflow |
| `src/lib/graph.ts` | Dependency and impact analysis |
| `src/components/ImpactMap.tsx` | Visualizes potential change impact |
| `src/components/InvariantPanel.tsx` | Displays identified invariants |
| `src/components/CounterexamplePanel.tsx` | Displays adversarial counterexamples |
| `src/components/VerificationPanel.tsx` | Presents verification information |
| `src/components/EvidencePanel.tsx` | Presents consolidated workflow evidence |
| `src/App.tsx` | Main application component |
| `src/main.tsx` | Application entry point |
| `src/index.css` | Application styling |

Additional implementation evidence and documentation are available in:

```text
docs/implementation-evidence/README.md
```

---

## Technology Stack

The project uses the following technologies and development tools.

| Technology | Role in the Project |
|---|---|
| React | Frontend user interface |
| TypeScript | Type-safe application development |
| Vite | Development server and frontend build tooling |
| Node.js | JavaScript runtime used by the project |
| AI-assisted development | Supports intent analysis and planning workflows |
| Git | Source code version control |
| GitHub | Repository hosting and collaboration |

---

## Getting Started

Follow these instructions to run DevPartner AI locally.

### Prerequisites

Make sure the following tools are installed:

- [Node.js](https://nodejs.org/)
- npm (included with Node.js)
- [Git](https://git-scm.com/)
- A code editor such as [Visual Studio Code](https://code.visualstudio.com/)

### 1. Clone the Repository

```bash
git clone https://github.com/Akash9250/Dev-Partner-Antigravity.git
```

### 2. Navigate to the Project Directory

```bash
cd Dev-Partner-Antigravity
```

### 3. Install Dependencies

```bash
npm install
```

This installs the dependencies declared in the project's `package.json` file.

### 4. Configure Environment Variables

If the project requires environment variables, create a local `.env` file using `.env.example` as a reference.

Add the required configuration values before starting the application.

See [Environment Variables](#environment-variables) for important security guidance.

### 5. Start the Development Server

```bash
npm run dev
```

Vite will start the local development server.

Open the URL displayed in your terminal. The default Vite development URL is:

```text
http://localhost:5173
```

> **Note:** The exact commands available for building, testing, and previewing the application depend on the scripts configured in `package.json`.

---

## Environment Variables

If environment variables are required by the application, configure them in a local `.env` file.

Use the following file as the configuration reference:

```text
.env.example
```

General setup:

1. Locate `.env.example` in the project root, if provided.
2. Create a local `.env` file.
3. Add the required environment-specific values.
4. Restart the development server after configuration.

### Security Best Practices

- Never commit API keys to GitHub.
- Do not expose private tokens or credentials in frontend code.
- Keep sensitive configuration in environment variables.
- Ensure local `.env` files are excluded from version control where appropriate.

The exact variable names and required values should be taken from the project's actual environment configuration.

---

## Demonstration

The project demonstration presents the DevPartner workflow, starting from a developer request and progressing through the different analysis and verification stages.

### Demonstration Flow

```text
Developer Request
       |
       v
Intent Extraction
       |
       v
Invariant Identification
       |
       v
Impact Analysis
       |
       v
Risk Assessment
       |
       v
Implementation Planning
       |
       v
Human Approval
       |
       v
Patch Application
       |
       v
Verification
       |
       v
Adversarial Counterexamples
       |
       v
Evidence Report
```

The demonstration focuses on the working application and how the different workflow stages are connected.

**Demo video:** Add your demonstration video link here.

---

## Project Evidence and Documentation

Supporting materials are organized into separate directories to make the implementation and development process easier to review.

### 1. Implementation Evidence

Location:

```text
docs/implementation-evidence/
```

This directory contains supporting evidence related to the implemented application and its functionality.

Additional documentation:

```text
docs/implementation-evidence/README.md
```

### 2. IBM Bob Development Evidence

Location:

```text
docs/ibm-bob-evidence/
```

This directory contains evidence related to the use of IBM Bob during development, including relevant task/session summaries and screenshots.

### 3. Additional Hackathon Documentation

```text
HACKATHON_EVIDENCE.md
```

Refer to this file for additional hackathon evidence guidance.

---

## IBM Bob Assisted Development

IBM Bob was used as an AI development assistant during the development and refinement of DevPartner AI.

It assisted with implementation, debugging, refinement, UI/component work, and integration across the central workflow.

The development work covered the following areas.

### 1. Core Workflow Pipeline

**Assisted task:** Connecting the different stages of request processing, analysis, planning, verification, and evidence generation.

**Related source files:**

- `src/hooks/usePipeline.ts`
- `src/App.tsx`
- `src/main.tsx`

### 2. AI-Assisted Analysis and Impact Mapping

**Assisted task:** Developer intent processing, implementation planning, dependency analysis, and understanding the potential impact of proposed code changes.

**Related source files:**

- `src/lib/ai.ts`
- `src/lib/graph.ts`
- `src/components/ImpactMap.tsx`

### 3. Verification and Adversarial Testing

**Assisted task:** Verification workflows, invariant checking, mutation testing, and displaying potential counterexamples.

**Related source files:**

- `src/lib/harness.ts`
- `src/components/InvariantPanel.tsx`
- `src/components/CounterexamplePanel.tsx`
- `src/components/VerificationPanel.tsx`

### 4. Frontend and Evidence Presentation

**Assisted task:** UI components for displaying analysis results, verification information, impact information, and evidence generated by the workflow.

**Related source files:**

- `src/components/EvidencePanel.tsx`
- `src/components/ImpactMap.tsx`
- `src/components/VerificationPanel.tsx`
- `src/index.css`

### Team Responsibilities

IBM Bob assisted with development activities, but the team remained responsible for:

- Product concept and design decisions.
- Application architecture.
- Feature decisions.
- Testing and validation.
- Code review.
- Final integration.

The resulting code was reviewed, tested, and integrated by the team member.

**Important:** IBM Bob development evidence is maintained separately from the application source code. Only actual IBM Bob task/session evidence should be included in the corresponding evidence directory.

---

## Vision

AI-assisted development should not require developers to blindly trust generated changes.

DevPartner AI aims to create a development workflow in which AI helps developers create and modify software while ensuring that proposed changes are understood, analyzed, reviewed, tested, challenged, and supported by evidence.

The broader vision is to move from code generation alone toward a more reliable and transparent development process.

```text
Understand
    |
    v
Analyze
    |
    v
Approve
    |
    v
Implement
    |
    v
Verify
    |
    v
Challenge
    |
    v
Generate Evidence
```

**Generate less blindly. Verify more intelligently.**

---

<p align="center">
  <strong>DevPartner AI</strong><br/>
  AI-assisted development with verification, transparency, and developer control.
</p>
