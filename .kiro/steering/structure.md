# Project Structure

## Directory Layout

```
.kiro/
├── specs/
│   └── SafeText/
│       ├── requirements.md    # User stories and acceptance criteria
│       ├── design.md          # Architecture and correctness properties
│       └── tasks.md           # Implementation checklist
└── steering/
    ├── product.md             # Product overview
    ├── tech.md                # Technology stack and conventions
    └── structure.md           # This file
```

## Application Architecture

The single HTML file follows an MVC-inspired pattern:

```
┌─────────────────────────────────────┐
│         User Interface              │
│  (HTML + Event Handlers + Display)  │
└──────────────┬──────────────────────┘
               │
               ▼
┌─────────────────────────────────────┐
│      Analysis Controller            │
│   (Orchestrates workflow)           │
└──────┬──────────────────┬───────────┘
       │                  │
       ▼                  ▼
┌─────────────┐    ┌─────────────┐
│   Pattern   │    │   Scoring   │
│  Detector   │    │   Engine    │
└─────────────┘    └─────────────┘
```

## Code Organization Within HTML File

When implementing, organize the JavaScript in this order:

1. **Configuration**: Pattern definitions, risk thresholds, recommendations
2. **Data Models**: Pattern, DetectedPattern, ScoreResult, AnalysisResult
3. **Core Logic**: Pattern detector, scoring engine, analysis controller
4. **UI Controller**: Event handlers, display functions, state management
5. **Initialization**: DOM ready handler, event wiring

## Key Components

- **Pattern Detector**: Regex-based pattern matching (high/medium/low risk)
- **Scoring Engine**: Point calculation and risk level determination
- **Analysis Controller**: Orchestrates detection → scoring → recommendations
- **UI Controller**: Manages DOM updates, user interactions, state resets
- **Results Formatter**: Transforms analysis data into user-friendly output

## Naming Conventions

- **Functions**: camelCase (e.g., `detectPatterns`, `calculateScore`)
- **Constants**: UPPER_SNAKE_CASE (e.g., `PATTERNS`, `RISK_THRESHOLDS`)
- **Data structures**: PascalCase for type names (e.g., `DetectedPattern`, `ScoreResult`)
- **DOM elements**: Descriptive IDs with hyphens (e.g., `message-input`, `check-button`)
