# SnapOps Refactor Plan

## Objective

Transform the current SnapOCR single-page application into a modular AI operations platform.

## Phase 1 — Stabilization

### State Management

**Current:**

- Global variables
- Module state
- DOM state

**Target:**

```
AppState
  |
  +-- OCR State
  +-- Workflow State
  +-- Settings State
  +-- Execution State
```

**Tasks:**

- Create AppState module
- Move extractedText into state
- Move currentImage into state
- Remove duplicate variables
- Add reset functions
- Add state change events

## Phase 2 — Extract Engines

Move logic away from UI.

Create:

### OCREngine

**Responsibilities:**

- Load Tesseract
- Process images
- Return OCR result
- Confidence scores

### AIEngine

**Responsibilities:**

- Model communication
- Prompt handling
- Streaming
- Errors
- Token usage

### PatternEngine

**Responsibilities:**

- Regex detection
- Device IDs
- Codes
- URLs
- Identifiers

### TriageEngine

**Responsibilities:**

**Input:**

- Context

**Output:**

- Classification
- Recommendations
- Workflow suggestion

## Phase 3 — Workflow System

Replace manual action selection.

**Current:**

- User selects actions

**Future:**

```
Context
   |
   v
Triage
   |
   v
Recommended Workflow
   |
   v
Execute
```

**Tasks:**

- WorkflowEngine
- ActionEngine
- Execution queue
- Retry handling
- AbortController support
- Execution logs

## Phase 4 — Storage Layer

Replace direct localStorage usage.

Create:

```
Storage.save()
Storage.load()
Storage.delete()
```

**Initial:**

- LocalStorage

**Future:**

- IndexedDB
- Cloud Database
- Enterprise Storage

## Phase 5 — UI Refactor

Split HTML into components.

**Target:**

- Dashboard
- ScreenshotPanel
- ContextPanel
- WorkflowPanel
- ExecutionPanel
- HistoryPanel

## Phase 6 — Security

Implement:

- HTML sanitization
- Input validation
- Regex validation
- API key protection
- Secure storage strategy
- Permission model

## Migration Rule

Do not rewrite everything.

Migrate incrementally:

```
Old Module
      |
      v
New Interface
      |
      v
Replace Implementation
```

Keep the application working after every step.

## First Refactor Tasks

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
