# Recruiter Shield Architecture

## Overview

Recruiter Shield is an automated agent that monitors LinkedIn messages, identifies recruiter communications, and blocks them before they reach your inbox. The system operates as a browser extension or background service that intercepts message events.

## System Components

### 1. Message Interceptor
- **Purpose**: Capture incoming LinkedIn messages before they display
- **Implementation**: Browser extension with content scripts
- **Technology**: Chrome Extension API / Webhook integration
- **Key Functions**:
  - Listen for WebSocket events from LinkedIn
  - Intercept message API calls
  - Parse message metadata (sender, content, connection level)

### 2. AI Detection Engine
- **Purpose**: Classify messages as recruiter/spam or legitimate
- **Implementation**: Local ML model or rule-based classifier
- **Features Analyzed**:
  - Keywords: "hiring", "opportunity", "job opening", "recruiter"
  - Sender profile: HR title, recruiting company
  - Message patterns: unsolicited outreach, template content
  - Connection level: 2nd/3rd degree connections
- **Output**: Probability score (0-1) + classification

### 3. Rule Engine
- **Purpose**: Apply user-defined blocking rules
- **Supported Rules**:
  - Block all messages from users with "recruiter" in title
  - Block messages containing specific keywords
  - Block from 2nd/3rd degree connections
  - Allow list for specific senders
  - Time-based blocking (business hours only)
- **Priority**: User rules > AI detection > default behavior

### 4. Action Handler
- **Purpose**: Execute blocking/alerting actions
- **Actions**:
  - Silent block (message never appears)
  - Auto-archive (move to archive without notification)
  - Alert notification (show summary to user)
  - Mark as spam
- **Execution**: Modify DOM, block API responses, or archive via LinkedIn API

### 5. Logging & Analytics
- **Purpose**: Track blocked messages and system performance
- **Data Collected**:
  - Number of messages blocked
  - False positive/negative rates
  - Common recruiter patterns
  - Time-based analytics
- **Storage**: Local SQLite database or encrypted JSON

### 6. Configuration Manager
- **Purpose**: Manage user settings and preferences
- **Settings**:
  - Blocking sensitivity level
  - Notification preferences
  - Keyword lists (block/allow)
  - Safe mode toggle
  - Export/import configuration

## Data Flow

```
LinkedIn Message
       ↓
Message Interceptor (WebSocket/API)
       ↓
Message Parser (extract metadata)
       ↓
Rule Engine (check user rules)
       ↓
AI Detection Engine (classify)
       ↓
Decision Matrix (rule + AI score)
       ↓
Action Handler (block/alert/archive)
       ↓
Logging (record action)
       ↓
User Notification (if enabled)
```

## Detection Strategies

### Pattern-Based Detection
- Common recruiter phrases: "exciting opportunity", "we're hiring", "role that fits"
- Job posting URLs: linkedin.com/jobs, greenhouse.io, lever.co
- HR/recruiter keywords in profile
- Template message signatures

### Profile Analysis
- Sender's job title contains "recruiter", "talent acquisition", "sourcing"
- Company is a recruitment agency
- High connection count with low engagement rate
- Profile shows recruitment activity

### Behavioral Analysis
- Sender has sent similar messages to multiple users
- Message sent during business hours
- Bulk message patterns
- No prior interaction history

## Privacy & Security

### Data Handling
- All processing happens locally (client-side)
- No message content sent to external servers
- Credentials stored in encrypted local storage
- Option to run entirely offline after setup

### LinkedIn API Considerations
- Uses official LinkedIn API where possible
- Respects rate limits and terms of service
- Implements proper authentication flow
- Handles session management securely

## Implementation Options

### Option 1: Browser Extension (Recommended)
- **Pros**: Real-time interception, no backend needed, easy to install
- **Cons**: Requires browser, limited to web LinkedIn
- **Tech Stack**: Chrome Extension, JavaScript, Manifest V3

### Option 2: Desktop Application
- **Pros**: Works outside browser, more control
- **Cons**: Complex setup, OS-specific
- **Tech Stack**: Electron, Node.js, Puppeteer

### Option 3: Webhook Service
- **Pros**: Platform-independent, remote monitoring
- **Cons**: Requires server, potential latency
- **Tech Stack**: Node.js, Express, LinkedIn API webhooks

## Future Enhancements

- Integration with LinkedIn's official API for direct blocking
- Machine learning model training on user feedback
- Community-shared recruiter patterns database
- Mobile app support
- Custom notification channels (Slack, email, SMS)
- Analytics dashboard with trends and insights

## Development Roadmap

### Phase 1: MVP
- Basic browser extension
- Keyword-based detection
- Manual blocking rules
- Simple logging

### Phase 2: AI Integration
- Local ML model for classification
- Behavioral pattern detection
- Profile analysis
- Improved accuracy

### Phase 3: Advanced Features
- Custom notification system
- Analytics dashboard
- Community patterns
- Multi-platform support

## Limitations

- LinkedIn may update their API/message system
- Browser extension limitations (mobile LinkedIn app)
- Risk of false positives blocking legitimate messages
- LinkedIn terms of service compliance needed
- Potential detection by LinkedIn as automated activity

## Mitigation Strategies

- Implement safe mode for reviewing blocked messages
- Regular updates to adapt to LinkedIn changes
- User feedback loop to improve detection
- Respect rate limits to avoid account flags
- Open source for community auditing
