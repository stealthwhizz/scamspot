# SafeText Design Document

## Overview

SafeText is a client-side web application that analyzes SMS messages for scam indicators using pattern matching and scoring algorithms. The application is built as a single, self-contained HTML file with embedded CSS and vanilla JavaScript, requiring no backend infrastructure or external dependencies. The architecture follows a simple MVC-inspired pattern where the UI layer handles user interactions, the analysis engine processes messages through pattern detection, and the scoring system calculates risk levels based on weighted pattern matches.

## Architecture

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────┐
│                     User Interface                       │
│  (HTML Structure + Event Handlers + Display Logic)      │
└────────────────┬────────────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────────────┐
│                  Analysis Controller                     │
│         (Orchestrates analysis workflow)                 │
└────────┬────────────────────────────────┬────────────────┘
         │                                │
         ▼                                ▼
┌──────────────────────┐      ┌──────────────────────────┐
│  Pattern Detector    │      │    Scoring Engine        │
│  (Regex matching)    │      │  (Point calculation)     │
└──────────────────────┘      └──────────────────────────┘
         │                                │
         └────────────┬───────────────────┘
                      ▼
         ┌────────────────────────────┐
         │     Results Formatter      │
         │  (Risk level + UI output)  │
         └────────────────────────────┘
```

### Component Responsibilities

1. **UI Layer**: Manages DOM elements, user input, button clicks, and result display
2. **Analysis Controller**: Coordinates the analysis workflow from input to output
3. **Pattern Detector**: Applies regex patterns to identify scam indicators
4. **Scoring Engine**: Calculates risk scores based on detected patterns
5. **Results Formatter**: Transforms analysis data into user-friendly output

## Components and Interfaces

### 1. Pattern Detector

**Purpose**: Identifies scam patterns in message text using regular expressions.

**Interface**:
```javascript
{
  detectPatterns(messageText: string): DetectedPattern[]
}
```

**Data Structures**:
```javascript
Pattern {
  name: string,           // Human-readable pattern name
  regex: RegExp,          // Regular expression for matching
  points: number,         // Score impact (-10, 15, or 25)
  category: string,       // "high", "medium", or "low"
  explanation: string     // User-facing explanation
}

DetectedPattern {
  name: string,
  points: number,
  category: string,
  explanation: string,
  matches: number         // Count of occurrences
}
```

### 2. Scoring Engine

**Purpose**: Calculates risk scores and determines risk levels.

**Interface**:
```javascript
{
  calculateScore(detectedPatterns: DetectedPattern[]): ScoreResult
}
```

**Data Structures**:
```javascript
ScoreResult {
  totalPoints: number,      // Raw point total
  percentage: number,       // 0-100 risk percentage
  riskLevel: string,        // "LOW", "MEDIUM", or "HIGH"
  riskColor: string,        // "green", "yellow", or "red"
  riskMessage: string       // User-facing risk description
}
```

### 3. Analysis Controller

**Purpose**: Orchestrates the complete analysis workflow.

**Interface**:
```javascript
{
  analyzeMessage(messageText: string): AnalysisResult
}
```

**Data Structures**:
```javascript
AnalysisResult {
  score: ScoreResult,
  detectedPatterns: DetectedPattern[],
  recommendations: string[],
  scammerGoals: string[]
}
```

### 4. UI Controller

**Purpose**: Manages all user interface interactions and updates.

**Interface**:
```javascript
{
  initialize(): void,
  handleAnalyzeClick(): void,
  displayResults(result: AnalysisResult): void,
  clearResults(): void,
  handleShare(): void
}
```

## Data Models

### Pattern Configuration

The application maintains a static configuration of all detection patterns:

```javascript
const PATTERNS = {
  highRisk: [
    {
      name: "Sensitive Information Request",
      regex: /\b(otp|pin|password|cvv|card number|account number)\b/gi,
      points: 25,
      explanation: "Requests for OTP, PIN, password, or CVV"
    },
    {
      name: "Urgency Language",
      regex: /\b(immediately|urgent|within \d+ hours?|account blocked|will expire|expires soon|act now)\b/gi,
      points: 25,
      explanation: "Creates false sense of urgency"
    },
    {
      name: "Threats",
      regex: /\b(legal action|warrant|police|court case|arrest|suspended|terminated)\b/gi,
      points: 25,
      explanation: "Uses threats or intimidation"
    },
    {
      name: "Prize Claims",
      regex: /\b(you won|lottery|winner|claim prize|congratulations|selected|reward)\b/gi,
      points: 25,
      explanation: "Claims you won something you didn't enter"
    },
    {
      name: "Financial Requests",
      regex: /\b(send money|pay now|transfer funds|payment required|make payment)\b/gi,
      points: 25,
      explanation: "Requests immediate payment or money transfer"
    }
  ],
  mediumRisk: [
    {
      name: "Bank Impersonation",
      regex: /\b(bank|banking|account|debit card|credit card)\b/gi,
      points: 15,
      explanation: "Claims to be from a bank without official sender ID"
    },
    {
      name: "Government Impersonation",
      regex: /\b(income tax|aadhaar|pan card|government|tax department|irs)\b/gi,
      points: 15,
      explanation: "Impersonates government agencies"
    },
    {
      name: "Suspicious Links",
      regex: /\b(bit\.ly|tinyurl|goo\.gl|t\.co|ow\.ly|is\.gd)\b/gi,
      points: 15,
      explanation: "Contains shortened or suspicious URLs"
    },
    {
      name: "Generic Greetings",
      regex: /\b(dear customer|dear user|dear member|valued customer)\b/gi,
      points: 15,
      explanation: "Uses generic greetings instead of your name"
    }
  ],
  lowRisk: [
    {
      name: "Official Sender Code",
      regex: /\b(BK-|VM-|AMAZON|GOOGLE|PAYPAL|NETFLIX)\b/g,
      points: -10,
      explanation: "Contains known official sender code"
    },
    {
      name: "Security Warning",
      regex: /\b(do not share|never share|keep confidential|for security)\b/gi,
      points: -10,
      explanation: "Includes legitimate security warnings"
    }
  ]
};
```

### Risk Level Thresholds

```javascript
const RISK_THRESHOLDS = {
  LOW: { max: 30, color: "green", message: "Likely legitimate" },
  MEDIUM: { min: 31, max: 60, color: "yellow", message: "Proceed with caution" },
  HIGH: { min: 61, color: "red", message: "Likely scam - do not respond" }
};
```

### Recommendations by Risk Level

```javascript
const RECOMMENDATIONS = {
  LOW: [
    "This message appears legitimate",
    "Still verify sender if requesting sensitive actions",
    "When in doubt, contact the organization directly"
  ],
  MEDIUM: [
    "Verify the sender through official channels",
    "Do not click any links in the message",
    "Contact the organization using known contact information",
    "Be cautious about sharing any information"
  ],
  HIGH: [
    "Do NOT respond to this message",
    "Do NOT click any links",
    "Do NOT share any personal information",
    "Block the sender",
    "Report as spam to your carrier",
    "Delete the message"
  ]
};
```


## Correctness Properties

*A property is a characteristic or behavior that should hold true across all valid executions of a system—essentially, a formal statement about what the system should do. Properties serve as the bridge between human-readable specifications and machine-verifiable correctness guarantees.*

### Property 1: Pattern detection completeness

*For any* message text and any pattern in the pattern configuration, if the pattern's regex matches the message, then the pattern should appear in the detected patterns list.

**Validates: Requirements 2.1, 2.2, 2.3**

### Property 2: Score normalization bounds

*For any* message text, the calculated risk percentage should be a value between 0 and 100 inclusive.

**Validates: Requirements 1.3, 6.4**

### Property 3: Risk level classification correctness

*For any* risk score, the assigned risk level should be "LOW" with green color for scores 0-30, "MEDIUM" with yellow color for scores 31-60, and "HIGH" with red color for scores 61-100.

**Validates: Requirements 3.1, 3.2, 3.3**

### Property 4: Pattern scoring accuracy

*For any* detected pattern, the points contributed to the total score should be exactly 25 for high-risk patterns, 15 for medium-risk patterns, and -10 for low-risk indicators, multiplied by the number of occurrences.

**Validates: Requirements 6.1, 6.2, 6.3, 6.5**

### Property 5: Case-insensitive pattern matching

*For any* message text and any variation of that text with different capitalization, the same patterns should be detected in both versions.

**Validates: Requirements 7.5**

### Property 6: High-risk pattern detection

*For any* message containing terms like "OTP", "PIN", "password", "CVV", "immediately", "account blocked", "legal action", "warrant", "you won", "lottery", "send money", or "pay now", the appropriate high-risk pattern should be detected.

**Validates: Requirements 7.1, 7.2, 7.3, 7.4, 8.1, 8.2, 9.1, 9.2**

### Property 7: Medium-risk pattern detection

*For any* message containing terms like "bank", "account", "Income Tax", "Aadhaar", "bit.ly", "tinyurl", "Dear customer", or "Dear user", the appropriate medium-risk pattern should be detected.

**Validates: Requirements 10.1, 10.2, 10.3, 11.1**

### Property 8: Low-risk indicator detection

*For any* message containing known official sender codes like "BK-", "VM-", "AMAZON" or security warnings like "do not share", the appropriate low-risk indicator should be detected and reduce the score.

**Validates: Requirements 12.1, 12.4**

### Property 9: Pattern robustness with punctuation

*For any* pattern and any message containing that pattern with various punctuation marks or word boundaries around it, the pattern should still be detected.

**Validates: Requirements 8.5, 10.5**

### Property 10: Result explanations completeness

*For any* analysis result with detected patterns, each detected pattern in the output should include an explanation field that is non-empty.

**Validates: Requirements 2.4**

### Property 11: Risk-appropriate recommendations

*For any* analysis result, the recommendations provided should match the risk level (LOW, MEDIUM, or HIGH) of the result.

**Validates: Requirements 3.4**

### Property 12: State reset completeness

*For any* application state after an analysis, invoking the clear/reset function should result in an empty input field, no displayed results, and no detected patterns.

**Validates: Requirements 5.2, 5.5**

### Property 13: Multiple analysis independence

*For any* sequence of two different messages analyzed consecutively, the results of the second analysis should not be affected by the first analysis.

**Validates: Requirements 5.4**

### Property 14: Input preservation

*For any* text input into the message field, the stored value should exactly match the input text.

**Validates: Requirements 1.1**

## Error Handling

### Input Validation

1. **Empty Input**: When the user attempts to analyze an empty message, display a friendly error message: "Please enter a message to analyze"
2. **Whitespace-Only Input**: Treat messages containing only whitespace as empty
3. **Very Long Input**: Accept messages up to 10,000 characters; truncate longer messages with a warning

### Pattern Matching Errors

1. **Regex Failures**: If a regex pattern fails to compile or execute, log the error to console and skip that pattern (fail gracefully)
2. **Invalid Pattern Configuration**: Validate pattern configuration on initialization; throw error if patterns are malformed

### UI Errors

1. **DOM Element Not Found**: Check for required DOM elements on initialization; display error if critical elements are missing
2. **Event Handler Failures**: Wrap event handlers in try-catch blocks to prevent UI freezing

### Scoring Edge Cases

1. **Negative Scores**: If total points are negative, set percentage to 0
2. **Excessive Scores**: If total points exceed reasonable bounds, cap percentage at 100
3. **NaN/Infinity**: Validate all numeric calculations; default to 0 if invalid

## Testing Strategy

SafeText will employ a dual testing approach combining unit tests and property-based tests to ensure comprehensive correctness validation.

### Unit Testing

Unit tests will verify specific examples, edge cases, and integration points:

1. **Pattern Detection Examples**:
   - Test known scam messages return HIGH risk
   - Test known legitimate messages return LOW risk
   - Test edge cases like empty input, whitespace-only input

2. **UI Integration**:
   - Test button click handlers trigger analysis
   - Test results display correctly in DOM
   - Test clear functionality resets state

3. **Accessibility**:
   - Test ARIA labels are present
   - Test keyboard navigation works
   - Test focus management

4. **Architecture Constraints**:
   - Test no external dependencies are loaded
   - Test file is self-contained
   - Test file size is under 100KB

### Property-Based Testing

Property-based tests will verify universal properties across all inputs using **fast-check** (JavaScript property-based testing library).

**Configuration**: Each property-based test will run a minimum of 100 iterations to ensure thorough coverage of the input space.

**Tagging Convention**: Each property-based test will include a comment tag in this exact format:
```javascript
// Feature: SafeText, Property {number}: {property description}
```

**Property Test Requirements**:
1. Each correctness property from the design document will be implemented as a SINGLE property-based test
2. Tests will be placed close to implementation to catch errors early
3. Tests will use smart generators that constrain to valid input spaces

**Property Tests to Implement**:

1. **Property 1: Pattern detection completeness** - Generate random messages with known patterns embedded, verify all are detected
2. **Property 2: Score normalization bounds** - Generate random messages, verify score is always 0-100
3. **Property 3: Risk level classification correctness** - Generate random scores, verify correct risk level assignment
4. **Property 4: Pattern scoring accuracy** - Generate messages with known pattern counts, verify point calculation
5. **Property 5: Case-insensitive pattern matching** - Generate messages with random capitalization, verify same detection
6. **Property 6: High-risk pattern detection** - Generate messages with high-risk terms, verify detection
7. **Property 7: Medium-risk pattern detection** - Generate messages with medium-risk terms, verify detection
8. **Property 8: Low-risk indicator detection** - Generate messages with low-risk indicators, verify detection and score reduction
9. **Property 9: Pattern robustness with punctuation** - Generate patterns with random punctuation, verify detection
10. **Property 10: Result explanations completeness** - Generate random analyses, verify all patterns have explanations
11. **Property 11: Risk-appropriate recommendations** - Generate random risk levels, verify matching recommendations
12. **Property 12: State reset completeness** - Generate random states, verify complete reset
13. **Property 13: Multiple analysis independence** - Generate pairs of messages, verify no cross-contamination
14. **Property 14: Input preservation** - Generate random text inputs, verify exact storage

### Test Generators

Custom generators will be created for property-based testing:

```javascript
// Generate messages with specific patterns
const messageWithPattern = (pattern) => fc.string().map(s => s + " " + pattern + " " + s);

// Generate risk scores in specific ranges
const lowRiskScore = () => fc.integer({ min: 0, max: 30 });
const mediumRiskScore = () => fc.integer({ min: 31, max: 60 });
const highRiskScore = () => fc.integer({ min: 61, max: 100 });

// Generate messages with random capitalization
const randomCapitalization = (text) => fc.array(fc.boolean(), { minLength: text.length, maxLength: text.length })
  .map(caps => text.split('').map((c, i) => caps[i] ? c.toUpperCase() : c.toLowerCase()).join(''));

// Generate messages with patterns and punctuation
const patternWithPunctuation = (pattern) => fc.constantFrom('.', ',', '!', '?', ';', ' ', '')
  .chain(before => fc.constantFrom('.', ',', '!', '?', ';', ' ', '')
    .map(after => before + pattern + after));
```

## Implementation Notes

### Performance Considerations

1. **Regex Compilation**: Compile all regex patterns once during initialization, not on every analysis
2. **DOM Updates**: Batch DOM updates to minimize reflows
3. **Debouncing**: Consider debouncing analysis if adding real-time checking

### Browser Compatibility

1. **ES6 Features**: Use ES6 features (arrow functions, template literals, const/let) supported by all modern browsers
2. **Regex Features**: Avoid advanced regex features not supported in Safari
3. **CSS Grid/Flexbox**: Use flexbox for layout (better browser support than grid)

### Accessibility Best Practices

1. **Semantic HTML**: Use semantic elements (button, main, section) for better screen reader support
2. **Focus Management**: Ensure focus moves logically after actions
3. **Color Contrast**: Ensure text meets WCAG AA standards (4.5:1 for normal text)
4. **Keyboard Shortcuts**: Consider adding keyboard shortcuts for power users

### Mobile Optimization

1. **Touch Targets**: Minimum 44x44px for all interactive elements
2. **Viewport Meta Tag**: Include proper viewport meta tag for responsive behavior
3. **Font Scaling**: Use relative units (rem, em) for better text scaling
4. **Reduced Motion**: Respect prefers-reduced-motion media query

### Security Considerations

1. **XSS Prevention**: Sanitize user input before displaying (use textContent, not innerHTML)
2. **No External Resources**: Avoid loading any external resources that could be compromised
3. **No Data Collection**: Ensure no user data is sent anywhere or stored persistently
