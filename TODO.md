SnapOps.

1. SNAPOPS_ARCHITECTURE.md — target architecture and system boundaries
2. SNAPOPS_REFACTOR_PLAN.md — step-by-step migration strategy from current codebase
3. SNAPOPS_ROADMAP.md — product roadmap replacing the old TODO mindset

You can put these directly into a /docs folder.

⸻

SnapOps Architecture

Vision

SnapOps is an AI-powered operations intelligence platform for analyzing screenshots, device states, logs, and operational signals.

OCR is only one input method.

The platform goal:

Input
 |
 v
Knowledge Extraction
 |
 v
Context Understanding
 |
 v
AI Triage
 |
 v
Workflow Automation
 |
 v
Action Execution
 |
 v
Operational History

⸻

Core Architecture

High Level

                 SnapOps
                    |
        +-----------+-----------+
        |                       |
   Input Layer             User Interface
        |                       |
        v                       v
+---------------------------------------+
|          Intelligence Layer           |
+---------------------------------------+
 OCR Engine
 Vision Engine
 Pattern Engine
 Context Engine
 AI Engine
 Triage Engine
                    |
                    v
+---------------------------------------+
|          Automation Layer             |
+---------------------------------------+
 Workflow Engine
 Action Engine
 Rule Engine
 Execution Queue
                    |
                    v
+---------------------------------------+
|          Platform Layer               |
+---------------------------------------+
 Storage
 Settings
 History
 Logging
 Integrations

⸻

Core Concepts

Context Object

The most important architectural change.

Never pass raw OCR text through the system.

Instead:

{
    id:"",
    timestamp:"",
    source:{
        type:"screenshot",
        filename:"",
    },
    device:{
        manufacturer:"",
        model:"",
        confidence:0
    },
    screen:{
        category:"",
        severity:"",
    },
    extracted:{
        text:"",
        codes:[],
        urls:[],
        identifiers:[]
    },
    ai:{
        summary:"",
        recommendations:[]
    },
    confidence:{
        overall:0
    }
}

Everything operates on Context.

⸻

Module Structure

Recommended future structure:

src/
├── app/
│   ├── AppState.js
│   ├── EventBus.js
│   └── Router.js
│
├── engines/
│   ├── OCREngine.js
│   ├── AIEngine.js
│   ├── TriageEngine.js
│   ├── ContextEngine.js
│   └── PatternEngine.js
│
├── automation/
│   ├── WorkflowEngine.js
│   ├── ActionEngine.js
│   └── RuleEngine.js
│
├── storage/
│   ├── Storage.js
│   ├── LocalStorage.js
│   └── Database.js
│
├── ui/
│   ├── Components
│   ├── Views
│   └── Modals
│
└── services/
    ├── NotificationService.js
    ├── LoggingService.js
    └── IntegrationService.js

⸻

Event Driven Model

Modules should communicate through events.

Example:

OCR_COMPLETE
      |
      v
CONTEXT_CREATED
      |
      v
TRIAGE_COMPLETE
      |
      v
WORKFLOW_SELECTED
      |
      v
ACTION_STARTED
      |
      v
ACTION_COMPLETED

Benefits:

* Easier testing
* Plugin support
* Less coupling
* Better debugging

⸻

Workflow Model

A workflow:

{
"name":"Samsung Provisioning",
"trigger":{
"type":"category",
"value":"PROVISIONING_SCREEN"
},
"steps":[
{
"action":"extract_code"
},
{
"action":"validate_device"
},
{
"action":"generate_report"
}
]
}

⸻

Action Model

Actions become reusable components.

Example:

{
"id":"extract_code",
"name":"Extract Provisioning Code",
"type":"extract",
"inputs":[
"context"
],
"outputs":[
"codes"
],
"timeout":5000
}

⸻

Long Term Vision

SnapOps should support:

* screenshots
* cameras
* browser extension
* log files
* APIs
* device telemetry
* emails
* cloud events

All inputs become Context.

All automation works on Context.

⸻

SnapOps Refactor Plan

Objective

Transform the current SnapOCR single-page application into a modular AI operations platform.

⸻

Phase 1 — Stabilization

State Management

Current:

Global variables
+
Module state
+
DOM state

Target:

AppState
 |
 +-- OCR State
 +-- Workflow State
 +-- Settings State
 +-- Execution State

Tasks:

* Create AppState module
* Move extractedText into state
* Move currentImage into state
* Remove duplicate variables
* Add reset functions
* Add state change events

⸻

Phase 2 — Extract Engines

Move logic away from UI.

Create:

OCREngine

Responsibilities:

* Load Tesseract
* Process images
* Return OCR result
* Confidence scores

AIEngine

Responsibilities:

* Model communication
* Prompt handling
* Streaming
* Errors
* Token usage

PatternEngine

Responsibilities:

* Regex detection
* Device IDs
* Codes
* URLs
* Identifiers

TriageEngine

Responsibilities:

Input:

Context

Output:

Classification
Recommendations
Workflow suggestion

⸻

Phase 3 — Workflow System

Replace manual action selection.

Current:

User selects actions

Future:

Context
↓
Triage
↓
Recommended Workflow
↓
Execute

Tasks:

* WorkflowEngine
* ActionEngine
* Execution queue
* Retry handling
* AbortController support
* Execution logs

⸻

Phase 4 — Storage Layer

Replace direct localStorage usage.

Create:

Storage.save()
Storage.load()
Storage.delete()

Initial:

LocalStorage

Future:

IndexedDB
Cloud Database
Enterprise Storage

⸻

Phase 5 — UI Refactor

Split HTML into components.

Target:

Dashboard
ScreenshotPanel
ContextPanel
WorkflowPanel
ExecutionPanel
HistoryPanel

⸻

Phase 6 — Security

Implement:

* HTML sanitization
* Input validation
* Regex validation
* API key protection
* Secure storage strategy
* Permission model

⸻

Migration Rule

Do not rewrite everything.

Migrate incrementally:

Old Module
      |
      v
New Interface
      |
      v
Replace Implementation

Keep the application working after every step.

⸻

First Refactor Tasks

Recommended order:

1. Create AppState
2. Create EventBus
3. Move OCR logic
4. Move AI logic
5. Create Context object
6. Create WorkflowEngine
7. Connect UI to engines
8. Remove old globals

This order gives maximum stability.

⸻

SnapOps Product Roadmap

Mission

Create an intelligent operations platform that understands device states and automatically performs troubleshooting workflows.

⸻

Version 2.0 — Foundation

Goal:

Stable architecture.

Features:

* Modular architecture
* Central state management
* Offline OCR
* Better error handling
* Improved themes
* Secure settings
* Clean workflow foundation

⸻

Version 2.1 — Intelligence Layer

Goal:

Understand operational situations.

Features:

* Context Engine
* Master Triage
* Device detection
* Severity classification
* Confidence scoring
* Pattern recognition
* AI recommendations

⸻

Version 2.2 — Automation Platform

Goal:

Automate troubleshooting.

Features:

* Workflow execution
* Conditional actions
* Action dependencies
* Retry policies
* Execution history
* Workflow templates
* Import/export

⸻

Version 3.0 — Operations Platform

Goal:

Manage real environments.

Features:

* Device fleets
* Dashboards
* Audit logs
* Reports
* Integrations
* Notifications
* Browser extension

⸻

Version 4.0 — Enterprise

Goal:

Multi-user operational intelligence.

Features:

* Teams
* Permissions
* Cloud synchronization
* Plugin ecosystem
* Marketplace
* Organization management

⸻

Core Product Principle

SnapOps should evolve from:

Screenshot OCR Tool

into:

AI Operations Intelligence Platform

OCR is the sensor.

AI is the reasoning layer.

Workflows are the automation layer.

⸻

also rename the repository structure and title and all old name related now:

SnapOCR/

to:

SnapOps/

and update:

* HTML title
* localStorage prefixes (snapocr_ → snapops_)
* module names
* README
* branding assets
* future package name
