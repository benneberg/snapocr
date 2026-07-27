# SnapOps Architecture

## Vision

SnapOps is an AI-powered operations intelligence platform for analyzing screenshots, device states, logs, and operational signals.

OCR is only one input method.

The platform goal:

```
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
```

## Core Architecture

### High Level

```
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
```

## Core Concepts

### Context Object

The most important architectural change.

Never pass raw OCR text through the system.

Instead:

```json
{
    "id": "",
    "timestamp": "",
    "source": {
        "type": "screenshot",
        "filename": ""
    },
    "device": {
        "manufacturer": "",
        "model": "",
        "confidence": 0
    },
    "screen": {
        "category": "",
        "severity": ""
    },
    "extracted": {
        "text": "",
        "codes": [],
        "urls": [],
        "identifiers": []
    },
    "ai": {
        "summary": "",
        "recommendations": []
    },
    "confidence": {
        "overall": 0
    }
}
```

Everything operates on Context.

## Module Structure

Recommended future structure:

```
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
```

## Event Driven Model

Modules should communicate through events.

Example:

```
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
```

Benefits:

- Easier testing
- Plugin support
- Less coupling
- Better debugging

## Workflow Model

A workflow:

```json
{
    "name": "Samsung Provisioning",
    "trigger": {
        "type": "category",
        "value": "PROVISIONING_SCREEN"
    },
    "steps": [
        {
            "action": "extract_code"
        },
        {
            "action": "validate_device"
        },
        {
            "action": "generate_report"
        }
    ]
}
```

## Action Model

Actions become reusable components.

Example:

```json
{
    "id": "extract_code",
    "name": "Extract Provisioning Code",
    "type": "extract",
    "inputs": [
        "context"
    ],
    "outputs": [
        "codes"
    ],
    "timeout": 5000
}
```

## Long Term Vision

SnapOps should support:

- screenshots
- cameras
- browser extension
- log files
- APIs
- device telemetry
- emails
- cloud events

All inputs become Context.

All automation works on Context.
