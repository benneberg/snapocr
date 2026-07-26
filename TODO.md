# SnapOCR 2.0 TODO
> Consolidated improvement roadmap based on comprehensive code review.
>
> Goal: Production-ready, enterprise-grade OCR + AI workflow platform.
---
# P0 — Critical (Fix Before Release)
## State Management
- [ ] Remove duplicate state variables (`extractedText`, `currentImage`)
- [ ] Choose a single source of truth (prefer `OCRModule` or centralized `AppState`)
- [ ] Eliminate global state duplication
- [ ] Add proper state reset methods
---
## OCR
- [ ] Disable **Extract Text** button after `clearImage()`
- [ ] Re-enable only after a valid image is selected
- [ ] Remove API-key requirement for OCR extraction
- [ ] Allow OCR to run completely offline
- [ ] Show API warning only when AI features are used
---
## Master Triage
- [ ] Connect Master Triage to Quick Mode
- [ ] Actually call `displayTriageResult()`
- [ ] Display detected:
  - device
  - severity
  - category
  - recommendations
- [ ] Remove dead UI if triage disabled
---
## Settings
- [ ] Improve Bootstrap modal initialization
- [ ] Use `bootstrap.Modal.getInstance()`
- [ ] Handle missing modal element safely
- [ ] Prevent initialization race conditions
---
## Error Handling
- [ ] Wrap `executeAction()` in try/catch
- [ ] Improve OCR error handling
- [ ] Improve workflow execution error handling
- [ ] Improve model loading error handling
- [ ] Improve Tesseract loading failures
- [ ] Add graceful fallback messages
---
## Theme Compatibility
Replace all hardcoded colors with CSS variables.
Replace:
- [ ] `color:white`
- [ ] `color:black`
- [ ] `rgba(255,255,255,...)`
With:
- [ ] `--text-primary`
- [ ] `--text-secondary`
- [ ] `--success-color`
- [ ] `--danger-color`
Ensure:
- [ ] Light mode readable
- [ ] Dark mode readable
---
# P1 — High Priority
## Workflow Execution
- [ ] Run workflows directly from Workflow Builder
- [ ] Connect Quick Mode to saved workflows
- [ ] Execute complete workflow instead of manually selecting actions
- [ ] Preserve execution order
- [ ] Continue / stop on error options
- [ ] Cancellation support (AbortController)
---
## Input Validation
Validate:
- [ ] Workflow names
- [ ] Action names
- [ ] Descriptions
- [ ] Regex patterns
- [ ] Empty workflows
- [ ] Duplicate names
- [ ] Missing prompts
---
## Security
- [ ] Escape all HTML output
- [ ] Sanitize rendered AI responses
- [ ] Validate regex before execution
- [ ] Prevent XSS in result rendering
---
## OCR Worker
- [ ] Reuse Tesseract worker
- [ ] Do not recreate worker every extraction
- [ ] Cleanup on page unload
- [ ] Cleanup on failures
---
## Model Handling
- [ ] Cache fetched model list
- [ ] Avoid unnecessary API reload after model selection
---
## Manual Text Input
- [ ] Add textarea for pasted text
- [ ] Allow AI analysis without OCR
- [ ] Support mixed OCR/manual workflows
---
## Replace Alerts
Replace:
- [ ] alert()
- [ ] confirm()
With:
- [ ] Toast notifications
- [ ] Confirmation dialogs
- [ ] Non-blocking UI
---
# P2 — UX Improvements
## Workflow Builder
- [ ] Drag-and-drop action ordering
- [ ] Move Up / Down buttons
- [ ] Edit custom actions
- [ ] Duplicate actions
- [ ] Clone workflows
- [ ] Delete confirmation
---
## Execution History
- [ ] Store execution history
- [ ] Timestamp runs
- [ ] Show previous OCR text
- [ ] Show previous AI results
- [ ] Clear history option
---
## Export / Import
Support:
- [ ] Export workflows
- [ ] Import workflows
- [ ] Export custom actions
- [ ] Version validation
- [ ] Merge vs Replace options
---
## Undo / Redo
- [ ] Workflow history
- [ ] Ctrl+Z
- [ ] Ctrl+Shift+Z
- [ ] History limit
---
## Search
- [ ] Debounce action search
- [ ] Cache filters
- [ ] Faster rendering
---
## Results Rendering
Instead of raw `<pre>`:
- [ ] Render markdown
- [ ] Headers
- [ ] Lists
- [ ] Bold
- [ ] Italics
- [ ] Code blocks
---
## Loading States
- [ ] Skeleton loaders
- [ ] Better OCR progress
- [ ] Better AI progress
- [ ] Animated placeholders
---
# P2 — Accessibility
## Keyboard Support
- [ ] Enter activates actions
- [ ] Space toggles selections
- [ ] Escape closes dialogs
- [ ] Tab navigation improvements
---
## ARIA
Add:
- [ ] aria-label
- [ ] aria-pressed
- [ ] aria-checked
- [ ] role attributes
---
## Focus Management
- [ ] Focus trap in modals
- [ ] Focus after tab switches
- [ ] Focus restoration
---
# P2 — Mobile
Improve layouts for:
- [ ] Pipeline
- [ ] Cards
- [ ] Buttons
- [ ] Navbar
- [ ] Upload area
- [ ] Settings button
- [ ] Modal spacing
---
# P2 — Visual Polish
## Hero Section
- [ ] Gradient background
- [ ] Animated grid
- [ ] Better hierarchy
- [ ] Product branding
---
## Upload Zone
- [ ] Hover animation
- [ ] Success state
- [ ] Better drag feedback
- [ ] Image preview overlay
---
## Action Library
- [ ] Action icons
- [ ] Category colors
- [ ] Better hierarchy
- [ ] Status indicators
---
## Dynamic UI
Add:
- [ ] Smooth transitions
- [ ] Animated progress
- [ ] Tab animations
- [ ] Loading effects
- [ ] Micro-interactions
---
# P3 — Performance
## Lazy Loading
- [ ] Load Tesseract on demand
- [ ] Lazy-load heavy libraries
- [ ] Reduce initial bundle size
---
## Rendering
- [ ] Reduce unnecessary DOM updates
- [ ] Optimize search rendering
- [ ] Cache repeated lookups
---
## HTML Escaping
- [ ] Replace DOM-based escaping with regex implementation
---
# P3 — Feature Enhancements
## Workflow Improvements
- [ ] Batch screenshot processing
- [ ] Workflow scheduling
- [ ] Workflow variables
- [ ] Conditional branching
- [ ] Retry policies
---
## Action Improvements
- [ ] Edit actions
- [ ] Duplicate actions
- [ ] Categories
- [ ] Favorites
- [ ] Templates
---
## Device Detection
- [ ] Detect from filename
- [ ] Detect from OCR
- [ ] Detect from metadata
- [ ] Merge confidence scores
---
## Browser Extension
- [ ] Right-click screenshot
- [ ] Send directly to SnapOCR
- [ ] Analyze instantly
---
## Audit Log
- [ ] Full execution log
- [ ] Searchable history
- [ ] Export reports
---
## Analytics (Optional)
- [ ] Usage metrics
- [ ] Workflow statistics
- [ ] Error frequency
---
# Testing
## Unit Tests
Cover:
- [ ] Device detection
- [ ] Provisioning detection
- [ ] Regex validation
- [ ] OCR helpers
- [ ] Workflow execution
- [ ] State management
---
## Integration Tests
- [ ] OCR pipeline
- [ ] AI pipeline
- [ ] Workflow execution
- [ ] Import/export
---
## Smoke Tests
- [ ] Startup
- [ ] Settings
- [ ] OCR
- [ ] AI
- [ ] Workflow save/load
---
# Documentation
## README
- [ ] Add screenshots
- [ ] Explain OCR vs AI requirements
- [ ] Update architecture diagram
- [ ] Document Quick Mode workflow execution
- [ ] Document Master Triage behavior
- [ ] Update roadmap
---
# Nice-to-Have
- [ ] Batch OCR
- [ ] Browser extension
- [ ] Workflow marketplace
- [ ] Plugin system
- [ ] Cloud sync
- [ ] Team collaboration
- [ ] Multi-language OCR
- [ ] Offline AI support
- [ ] Auto-update mechanism
---
# Release Goals
## Version 2.0.1
- Critical bug fixes
- State cleanup
- Theme fixes
- Better error handling
---
## Version 2.1
- Workflow execution
- Manual text input
- Export/import
- Accessibility
- Performance improvements
---
## Version 2.2
- Undo/Redo
- History
- Batch processing
- Visual redesign
---
## Version 3.0
- Browser extension
- Plugin architecture
- Team collaboration
- Cloud synchronization
- Enterprise features

This roadmap consolidates roughly 60–70 individual review items into ~15 major workstreams, removes duplicated recommendations (e.g., Tesseract worker reuse, debounce, accessibility, export/import, state management), and organizes them into a practical release plan. It should serve as a clean engineering backlog rather than a list of isolated review comments.
