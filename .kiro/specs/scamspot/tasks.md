 # Implementation Plan

- [x] 1. Create HTML structure and basic styling





  - Create single HTML file with semantic structure (header, main, footer)
  - Add viewport meta tag and basic responsive CSS
  - Create textarea for message input with proper labels
  - Create "Check for Scam" button with accessible markup
  - Create results display area with placeholders for risk badge, score, and red flags list
  - Add "Check Another Message" and "Share SafeText" buttons
  - Implement mobile-first responsive layout with flexbox
  - Apply color scheme (blue for trust, green/yellow/red for risk levels)
  - Ensure minimum font size of 16px and touch targets of 44x44px
  - Add ARIA labels and semantic HTML for accessibility
  - _Requirements: 4.3, 13.1, 13.4, 13.5, 14.2, 14.3, 14.4, 15.1, 15.3_

- [x] 2. Implement pattern detection system





  - [x] 2.1 Define pattern configuration data structure


    - Create PATTERNS object with high-risk, medium-risk, and low-risk categories
    - Define regex patterns for sensitive information requests (OTP, PIN, password, CVV)
    - Define regex patterns for urgency language (immediately, within X hours, account blocked, will expire)
    - Define regex patterns for threats (legal action, warrant, police, court case)
    - Define regex patterns for prize claims (you won, lottery, claim prize, congratulations)
    - Define regex patterns for financial requests (send money, pay now, transfer funds)
    - Define regex patterns for bank impersonation (bank, account, debit card, credit card)
    - Define regex patterns for government impersonation (Income Tax, Aadhaar, PAN card)
    - Define regex patterns for suspicious links (bit.ly, tinyurl, goo.gl)
    - Define regex patterns for generic greetings (Dear customer, Dear user)
    - Define regex patterns for official sender codes (BK-, VM-, AMAZON, GOOGLE)
    - Define regex patterns for security warnings (do not share, never share)
    - Ensure all patterns use case-insensitive flags
    - Include point values (25, 15, -10) and explanations for each pattern
    - _Requirements: 7.1, 7.2, 7.5, 8.1, 8.2, 9.1, 9.2, 10.1, 10.2, 10.3, 11.1, 12.1, 12.4_

  - [ ]* 2.2 Write property test for pattern detection completeness
    - **Property 1: Pattern detection completeness**
    - **Validates: Requirements 2.1, 2.2, 2.3**

  - [x] 2.3 Implement detectPatterns function

    - Create function that takes message text as input
    - Iterate through all pattern categories (high, medium, low risk)
    - Apply each regex pattern to the message text
    - Count occurrences of each pattern match
    - Return array of DetectedPattern objects with name, points, category, explanation, and match count
    - Handle regex errors gracefully by logging and skipping failed patterns
    - _Requirements: 2.1, 2.2, 2.3, 7.5, 8.5, 10.5_

  - [ ]* 2.4 Write property test for case-insensitive matching
    - **Property 5: Case-insensitive pattern matching**
    - **Validates: Requirements 7.5**

  - [ ]* 2.5 Write property test for high-risk pattern detection
    - **Property 6: High-risk pattern detection**
    - **Validates: Requirements 7.1, 7.2, 7.3, 7.4, 8.1, 8.2, 9.1, 9.2**

  - [ ]* 2.6 Write property test for medium-risk pattern detection
    - **Property 7: Medium-risk pattern detection**
    - **Validates: Requirements 10.1, 10.2, 10.3, 11.1**

  - [ ]* 2.7 Write property test for low-risk indicator detection
    - **Property 8: Low-risk indicator detection**
    - **Validates: Requirements 12.1, 12.4**

  - [ ]* 2.8 Write property test for pattern robustness with punctuation
    - **Property 9: Pattern robustness with punctuation**
    - **Validates: Requirements 8.5, 10.5**

- [-] 3. Implement scoring engine


  - [x] 3.1 Create calculateScore function



    - Accept array of DetectedPattern objects as input
    - Sum total points from all detected patterns (pattern.points × pattern.matches)
    - Convert total points to percentage (normalize to 0-100 range)
    - Handle negative scores by setting percentage to 0
    - Handle excessive scores by capping percentage at 100
    - Determine risk level based on thresholds (0-30 LOW, 31-60 MEDIUM, 61+ HIGH)
    - Assign appropriate color (green, yellow, red) and message for risk level
    - Return ScoreResult object with totalPoints, percentage, riskLevel, riskColor, riskMessage
    - _Requirements: 1.3, 3.1, 3.2, 3.3, 6.1, 6.2, 6.3, 6.4, 6.5_

  - [ ]* 3.2 Write property test for score normalization bounds
    - **Property 2: Score normalization bounds**
    - **Validates: Requirements 1.3, 6.4**

  - [ ]* 3.3 Write property test for risk level classification
    - **Property 3: Risk level classification correctness**
    - **Validates: Requirements 3.1, 3.2, 3.3**

  - [ ]* 3.4 Write property test for pattern scoring accuracy
    - **Property 4: Pattern scoring accuracy**
    - **Validates: Requirements 6.1, 6.2, 6.3, 6.5**

- [x] 4. Implement recommendations and scammer goals logic




  - [x] 4.1 Create recommendations configuration


    - Define RECOMMENDATIONS object with arrays for LOW, MEDIUM, and HIGH risk levels
    - LOW: "This message appears legitimate", "Still verify sender if requesting sensitive actions", "When in doubt, contact the organization directly"
    - MEDIUM: "Verify the sender through official channels", "Do not click any links", "Contact the organization using known contact information", "Be cautious about sharing any information"
    - HIGH: "Do NOT respond to this message", "Do NOT click any links", "Do NOT share any personal information", "Block the sender", "Report as spam to your carrier", "Delete the message"
    - _Requirements: 3.4_

  - [x] 4.2 Create getRecommendations function


    - Accept risk level as input
    - Return appropriate recommendations array based on risk level
    - _Requirements: 3.4_

  - [ ]* 4.3 Write property test for risk-appropriate recommendations
    - **Property 11: Risk-appropriate recommendations**
    - **Validates: Requirements 3.4**

  - [x] 4.4 Create getScammerGoals function


    - Accept array of detected patterns as input
    - Analyze pattern types to determine what scammers might want
    - Return array of scammer goal descriptions (e.g., "Steal your OTP or password", "Trick you into sending money", "Obtain your personal information")
    - _Requirements: 3.5_

  - [ ]* 4.5 Write property test for result explanations completeness
    - **Property 10: Result explanations completeness**
    - **Validates: Requirements 2.4**

- [x] 5. Implement analysis controller




  - [x] 5.1 Create analyzeMessage function


    - Accept message text as input
    - Validate input (check for empty or whitespace-only messages)
    - Call detectPatterns to get detected patterns
    - Call calculateScore with detected patterns to get score result
    - Call getRecommendations with risk level to get recommendations
    - Call getScammerGoals with detected patterns to get scammer goals
    - Return AnalysisResult object containing score, detectedPatterns, recommendations, scammerGoals
    - Handle errors gracefully and return appropriate error messages
    - _Requirements: 1.1, 1.2, 2.4, 3.5_

  - [ ]* 5.2 Write property test for input preservation
    - **Property 14: Input preservation**
    - **Validates: Requirements 1.1**

- [ ] 6. Implement UI controller and event handlers
  - [ ] 6.1 Create initialize function
    - Get references to all DOM elements (textarea, buttons, results area)
    - Validate that all required elements exist
    - Attach event listeners to buttons
    - Set initial focus on textarea
    - _Requirements: 1.1_

  - [ ] 6.2 Create handleAnalyzeClick function
    - Get message text from textarea
    - Show loading state or disable button during analysis
    - Call analyzeMessage with the text
    - Call displayResults with the analysis result
    - Handle empty input by showing error message
    - Wrap in try-catch to handle any errors gracefully
    - _Requirements: 1.2, 1.3, 1.4_

  - [ ] 6.3 Create displayResults function
    - Accept AnalysisResult as input
    - Update risk badge with color, level, and message
    - Display risk percentage prominently
    - Render list of detected red flags with explanations
    - Display recommendations as a list
    - Display scammer goals if applicable
    - Show "Check Another Message" button
    - Use textContent (not innerHTML) to prevent XSS
    - Handle case where no red flags are detected
    - _Requirements: 1.3, 1.4, 2.4, 2.5, 3.1, 3.2, 3.3, 3.4, 3.5, 5.1_

  - [ ] 6.4 Create clearResults function
    - Clear textarea value
    - Remove all displayed results from DOM
    - Reset risk badge and score display
    - Clear red flags list
    - Clear recommendations and scammer goals
    - Focus cursor on textarea
    - _Requirements: 5.2, 5.3, 5.5_

  - [ ]* 6.5 Write property test for state reset completeness
    - **Property 12: State reset completeness**
    - **Validates: Requirements 5.2, 5.5**

  - [ ]* 6.6 Write property test for multiple analysis independence
    - **Property 13: Multiple analysis independence**
    - **Validates: Requirements 5.4**

  - [ ] 6.7 Create handleShare function
    - Create share text with app description
    - Use Web Share API if available (for mobile)
    - Fallback to copying link to clipboard
    - Show confirmation message to user
    - _Requirements: 4.1, 4.2_

  - [ ] 6.8 Wire up all event handlers
    - Attach handleAnalyzeClick to "Check for Scam" button
    - Attach clearResults to "Check Another Message" button
    - Attach handleShare to "Share SafeText" button
    - Add keyboard support (Enter key in textarea triggers analysis)
    - Ensure all handlers work without page reload
    - _Requirements: 1.2, 4.1, 5.1, 5.4, 13.2_

- [ ]* 7. Create unit tests for edge cases and examples
  - Test empty input shows error message
  - Test whitespace-only input is treated as empty
  - Test known high-risk scam message: "Dear customer, your account will be blocked in 24 hours. Verify immediately: bit.ly/verify123 Share OTP to continue"
  - Test known low-risk legitimate message: "Your OTP for Amazon order is 456789. Valid for 10 minutes. Do not share with anyone. -AMAZON"
  - Test very long input (10,000+ characters) is handled
  - Test no external dependencies are loaded
  - Test file is self-contained
  - Test ARIA labels are present on interactive elements
  - Test keyboard navigation works (tab order, Enter key)
  - Test focus management after actions
  - _Requirements: 1.5, 4.3, 13.1, 13.2, 13.4, 13.5, 15.1, 15.2, 15.3_

- [ ] 8. Final polish and validation
  - Verify total file size is under 100KB
  - Test on multiple browsers (Chrome, Firefox, Safari, Edge)
  - Test on mobile devices (iOS Safari, Chrome Android)
  - Verify responsive layout works at various screen sizes
  - Check color contrast meets WCAG AA standards
  - Add smooth animations for results display
  - Add prefers-reduced-motion support
  - Verify all requirements are met
  - _Requirements: 14.1, 14.4, 15.4, 15.5_

- [ ] 9. Checkpoint - Ensure all tests pass
  - Ensure all tests pass, ask the user if questions arise.
