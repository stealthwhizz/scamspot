# Requirements Document

## Introduction

SafeText is a single-purpose web application designed to help users identify suspicious SMS messages by analyzing them for common scam patterns. The application provides instant risk assessment, detects specific red flags, and offers clear recommendations on appropriate actions. Built as a standalone HTML page with no backend dependencies, SafeText works completely offline and is optimized for mobile-first usage, making it accessible to users who need quick scam detection on their phones.

## Glossary

- **SafeText**: The SMS scam detection web application system
- **SMS**: Short Message Service, text messages sent to mobile phones
- **OTP**: One-Time Password, a temporary code used for authentication
- **PIN**: Personal Identification Number
- **CVV**: Card Verification Value, security code on payment cards
- **Risk Score**: A numerical value from 0-100 representing the probability that a message is a scam
- **Red Flag**: A suspicious pattern or indicator detected in a message that suggests scam activity
- **Sender ID**: The identifier shown as the sender of an SMS message
- **Pattern**: A specific text sequence or characteristic used to identify scam indicators

## Requirements

### Requirement 1

**User Story:** As a user, I want to paste a suspicious SMS message and receive instant analysis, so that I can quickly determine if it might be a scam.

#### Acceptance Criteria

1. WHEN a user pastes text into the message input area, THE SafeText SHALL accept and store the message content for analysis
2. WHEN a user clicks the check button, THE SafeText SHALL analyze the message and display results within 1 second
3. WHEN analysis is complete, THE SafeText SHALL display a risk score between 0 and 100 percent
4. WHEN analysis is complete, THE SafeText SHALL display a color-coded risk level badge based on the score
5. THE SafeText SHALL function without requiring internet connectivity or backend server communication

### Requirement 2

**User Story:** As a user, I want to see specific red flags detected in my message, so that I can understand exactly what makes it suspicious.

#### Acceptance Criteria

1. WHEN the SafeText analyzes a message, THE SafeText SHALL detect all high-risk patterns present in the message text
2. WHEN the SafeText analyzes a message, THE SafeText SHALL detect all medium-risk patterns present in the message text
3. WHEN the SafeText analyzes a message, THE SafeText SHALL detect all low-risk indicators present in the message text
4. WHEN displaying results, THE SafeText SHALL show a list of all detected red flags with explanatory text for each flag
5. WHEN no red flags are detected, THE SafeText SHALL display a message indicating the analysis found no suspicious patterns

### Requirement 3

**User Story:** As a user, I want clear recommendations on what action to take, so that I can respond appropriately to potentially dangerous messages.

#### Acceptance Criteria

1. WHEN the risk score is between 0 and 30 percent, THE SafeText SHALL display a green badge with "LOW RISK" label and "Likely legitimate" message
2. WHEN the risk score is between 31 and 60 percent, THE SafeText SHALL display a yellow badge with "MEDIUM RISK" label and "Proceed with caution" message
3. WHEN the risk score is 61 percent or higher, THE SafeText SHALL display a red badge with "HIGH RISK" label and "Likely scam - do not respond" message
4. WHEN displaying results, THE SafeText SHALL provide specific recommended actions based on the detected risk level
5. WHEN displaying results, THE SafeText SHALL explain what information scammers might be attempting to obtain

### Requirement 4

**User Story:** As a user, I want to share this tool with family members, so that they can also protect themselves from SMS scams.

#### Acceptance Criteria

1. WHEN viewing the application, THE SafeText SHALL display a share button that is clearly visible and accessible
2. WHEN a user clicks the share button, THE SafeText SHALL provide options to share via common social media platforms
3. THE SafeText SHALL function as a single HTML file that can be easily distributed and opened locally
4. THE SafeText SHALL maintain full functionality when opened from any location without requiring installation
5. WHEN accessed on mobile devices, THE SafeText SHALL display properly with touch-friendly interface elements

### Requirement 5

**User Story:** As a user, I want to check multiple messages in succession, so that I can analyze several suspicious texts efficiently.

#### Acceptance Criteria

1. WHEN results are displayed, THE SafeText SHALL show a button to check another message
2. WHEN the user clicks the check another message button, THE SafeText SHALL clear the previous results and reset the input area
3. WHEN the user clicks the check another message button, THE SafeText SHALL focus the cursor on the message input area
4. THE SafeText SHALL preserve the application state between multiple analyses without requiring page reload
5. WHEN clearing results, THE SafeText SHALL remove all previously displayed red flags and risk assessments

### Requirement 6

**User Story:** As a developer, I want the application to correctly calculate risk scores based on pattern detection, so that users receive accurate scam assessments.

#### Acceptance Criteria

1. WHEN a high-risk pattern is detected, THE SafeText SHALL add 25 points to the risk score for each occurrence
2. WHEN a medium-risk pattern is detected, THE SafeText SHALL add 15 points to the risk score for each occurrence
3. WHEN a low-risk indicator is detected, THE SafeText SHALL subtract 10 points from the risk score for each occurrence
4. WHEN calculating the final score, THE SafeText SHALL convert the total points to a percentage value between 0 and 100
5. WHEN multiple patterns of the same type are detected, THE SafeText SHALL count each occurrence separately in the score calculation

### Requirement 7

**User Story:** As a user, I want the application to detect requests for sensitive information, so that I can identify phishing attempts.

#### Acceptance Criteria

1. WHEN a message contains requests for OTP, THE SafeText SHALL flag it as a high-risk pattern
2. WHEN a message contains requests for PIN, password, or CVV, THE SafeText SHALL flag it as a high-risk pattern
3. WHEN a message contains the phrase "share OTP" or similar variations, THE SafeText SHALL flag it as a high-risk pattern
4. WHEN a message contains requests for bank account details, THE SafeText SHALL flag it as a high-risk pattern
5. THE SafeText SHALL perform case-insensitive pattern matching for all sensitive information requests

### Requirement 8

**User Story:** As a user, I want the application to detect urgency tactics and threats, so that I can recognize pressure-based scam techniques.

#### Acceptance Criteria

1. WHEN a message contains urgency language such as "immediately", "within 24 hours", "account blocked", or "will expire", THE SafeText SHALL flag it as a high-risk pattern
2. WHEN a message contains threat language such as "legal action", "warrant", "police will come", or "court case", THE SafeText SHALL flag it as a high-risk pattern
3. WHEN a message contains time-pressure phrases, THE SafeText SHALL flag it as a high-risk pattern
4. WHEN a message contains account suspension threats, THE SafeText SHALL flag it as a high-risk pattern
5. THE SafeText SHALL detect urgency and threat patterns regardless of word boundaries or punctuation

### Requirement 9

**User Story:** As a user, I want the application to detect prize and financial scams, so that I can avoid "too good to be true" schemes.

#### Acceptance Criteria

1. WHEN a message contains prize claim language such as "you won", "lottery winner", "claim prize", or "congratulations", THE SafeText SHALL flag it as a high-risk pattern
2. WHEN a message contains financial request language such as "send money", "pay now", or "transfer funds", THE SafeText SHALL flag it as a high-risk pattern
3. WHEN a message claims unexpected winnings or prizes, THE SafeText SHALL flag it as a high-risk pattern
4. WHEN a message requests immediate payment, THE SafeText SHALL flag it as a high-risk pattern
5. THE SafeText SHALL detect prize and financial scam patterns with case-insensitive matching

### Requirement 10

**User Story:** As a user, I want the application to detect impersonation attempts, so that I can identify fake messages claiming to be from legitimate organizations.

#### Acceptance Criteria

1. WHEN a message claims to be from a bank without an official sender ID, THE SafeText SHALL flag it as a medium-risk pattern
2. WHEN a message impersonates government entities such as "Income Tax", "Aadhaar", or "PAN card", THE SafeText SHALL flag it as a medium-risk pattern
3. WHEN a message uses generic greetings like "Dear customer" or "Dear user", THE SafeText SHALL flag it as a medium-risk pattern
4. WHEN a message contains government agency names without official sender codes, THE SafeText SHALL flag it as a medium-risk pattern
5. THE SafeText SHALL detect impersonation patterns regardless of capitalization or spacing variations

### Requirement 11

**User Story:** As a user, I want the application to detect suspicious links and poor message quality, so that I can identify unprofessional scam attempts.

#### Acceptance Criteria

1. WHEN a message contains shortened URLs from services like "bit.ly" or "tinyurl", THE SafeText SHALL flag it as a medium-risk pattern
2. WHEN a message contains links to unfamiliar or suspicious domains, THE SafeText SHALL flag it as a medium-risk pattern
3. WHEN a message contains multiple spelling errors, THE SafeText SHALL flag it as a medium-risk pattern
4. WHEN a message contains random capitalization patterns, THE SafeText SHALL flag it as a medium-risk pattern
5. THE SafeText SHALL detect URL patterns including common URL shortening services and suspicious domain structures

### Requirement 12

**User Story:** As a user, I want the application to recognize legitimate message indicators, so that I don't get false positives for genuine communications.

#### Acceptance Criteria

1. WHEN a message contains known official sender codes such as "BK-", "VM-", or "AMAZON", THE SafeText SHALL reduce the risk score by 10 points
2. WHEN a message demonstrates professional formatting, THE SafeText SHALL reduce the risk score by 10 points
3. WHEN a message contains no suspicious links or requests, THE SafeText SHALL apply appropriate low-risk scoring
4. WHEN a message includes legitimate security warnings like "Do not share with anyone", THE SafeText SHALL recognize it as a low-risk indicator
5. THE SafeText SHALL balance positive and negative indicators to produce an accurate overall risk assessment

### Requirement 13

**User Story:** As a user with accessibility needs, I want the application to be fully accessible, so that I can use it with assistive technologies.

#### Acceptance Criteria

1. WHEN the application loads, THE SafeText SHALL include ARIA labels for all interactive elements
2. WHEN a user navigates with keyboard only, THE SafeText SHALL allow access to all functionality without requiring a mouse
3. WHEN using screen readers, THE SafeText SHALL announce results and risk levels clearly
4. THE SafeText SHALL maintain a logical tab order through all interactive elements
5. WHEN displaying color-coded risk levels, THE SafeText SHALL also use text labels and icons to convey information without relying solely on color

### Requirement 14

**User Story:** As a mobile user, I want the application to work perfectly on my phone, so that I can check messages immediately when I receive them.

#### Acceptance Criteria

1. WHEN accessed on a mobile device, THE SafeText SHALL display with a responsive layout optimized for small screens
2. WHEN accessed on a mobile device, THE SafeText SHALL use touch-friendly button sizes of at least 44x44 pixels
3. WHEN accessed on a mobile device, THE SafeText SHALL use font sizes of at least 16 pixels for body text
4. WHEN the viewport width is less than 768 pixels, THE SafeText SHALL stack elements vertically for optimal mobile viewing
5. THE SafeText SHALL load and function within 3 seconds on mobile devices with standard 4G connectivity

### Requirement 15

**User Story:** As a developer, I want the application to be built with vanilla JavaScript in a single file, so that it's lightweight, portable, and easy to maintain.

#### Acceptance Criteria

1. THE SafeText SHALL be implemented as a single HTML file containing all code and styles
2. THE SafeText SHALL use vanilla JavaScript without requiring external frameworks or libraries
3. THE SafeText SHALL embed all CSS styles within the HTML file
4. THE SafeText SHALL have a total file size of less than 100 kilobytes
5. THE SafeText SHALL function correctly on all modern browsers including Chrome, Firefox, Safari, and Edge
