# Phase 2 Feedback System - Component Architecture

## Visual Component Hierarchy

```
┌─────────────────────────────────────────────────────────────────┐
│                     Singura Application                          │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ├─── Automations View
                              │         │
        ┌─────────────────────┼─────────┴──────────────────────┐
        │                     │                                 │
        ▼                     ▼                                 ▼
┌──────────────┐    ┌──────────────┐              ┌──────────────────┐
│ Automation   │    │ Automation   │              │  Automation      │
│ Card 1       │    │ Card 2       │              │  Details Modal   │
└──────────────┘    └──────────────┘              └──────────────────┘
        │                   │                              │
        │                   │                              │
        ▼                   ▼                              ▼
┌──────────────┐    ┌──────────────┐         ┌────────────────────────┐
│ Automation   │    │ Automation   │         │      Modal Tabs        │
│ Feedback     │    │ Feedback     │         │ ┌──────┬──────┬──────┐│
│ (Compact)    │    │ (Compact)    │         │ │Perms │Risk  │Feed  ││
└──────────────┘    └──────────────┘         │ └──────┴──────┴──────┘│
        │                   │                 └────────────────────────┘
        │                   │                              │
        ▼                   ▼                              ▼
┌──────────────┐    ┌──────────────┐         ┌────────────────────────┐
│ Feedback     │    │ Feedback     │         │   Automation Feedback  │
│ Button       │    │ Button       │         │      (Full Mode)       │
│ 👍 👎       │    │ 👍 👎       │         └────────────────────────┘
└──────────────┘    └──────────────┘                      │
        │                   │                              │
        │ Click             │ Click                        ▼
        ▼                   ▼                    ┌──────────────────┐
┌──────────────┐    ┌──────────────┐           │  Feedback Button │
│ Feedback     │    │ Feedback     │           │      👍 👎      │
│ Form Modal   │    │ Form Modal   │           └──────────────────┘
└──────────────┘    └──────────────┘                      │
                                                           ▼
                                              ┌────────────────────────┐
                                              │    Feedback Form       │
                                              │ • Feedback Type        │
                                              │ • Comment              │
                                              │ • Suggested Corrections│
                                              └────────────────────────┘
                                                           │
                                                           ▼
                                              ┌────────────────────────┐
                                              │    Feedback List       │
                                              │ • All user feedback    │
                                              │ • History & status     │
                                              └────────────────────────┘
```

## Component Breakdown

### 1. AutomationCard (Modified)
```
┌────────────────────────────────────────┐
│ AutomationCard                         │
│ ┌────────────────────────────────────┐ │
│ │ Automation Name                    │ │
│ │ Type: Workflow | Risk: Medium      │ │
│ ├────────────────────────────────────┤ │
│ │ Description...                     │ │
│ ├────────────────────────────────────┤ │
│ │ [NEW] Feedback Section             │ │
│ │   AutomationFeedback (compact)     │ │
│ │   ┌──────────────────────────────┐ │ │
│ │   │  👍  👎  💬 1              │ │ │
│ │   └──────────────────────────────┘ │ │
│ ├────────────────────────────────────┤ │
│ │ [View Details] [Disable]           │ │
│ └────────────────────────────────────┘ │
└────────────────────────────────────────┘
```

### 2. AutomationDetailsModal (Modified)
```
┌──────────────────────────────────────────────────────────┐
│ AutomationDetailsModal                                   │
│ ┌──────────────────────────────────────────────────────┐ │
│ │ Automation Name                    [Assess Risk] [×] │ │
│ ├──────────────────────────────────────────────────────┤ │
│ │ [Permissions] [Risk] [Feedback] [Details]            │ │
│ ├──────────────────────────────────────────────────────┤ │
│ │ [NEW] Feedback Tab Content:                          │ │
│ │ ┌──────────────────────────────────────────────────┐ │ │
│ │ │ Detection Feedback                               │ │ │
│ │ │ ┌──────────────────────────────────────────────┐ │ │ │
│ │ │ │ AutomationFeedback (full mode)               │ │ │ │
│ │ │ │   👍 Correct  👎 Incorrect                  │ │ │ │
│ │ │ │                                              │ │ │ │
│ │ │ │ [Expanded Form]                              │ │ │ │
│ │ │ │ • Feedback Type                              │ │ │ │
│ │ │ │ • Comment                                    │ │ │ │
│ │ │ │ • Suggested Corrections                      │ │ │ │
│ │ │ └──────────────────────────────────────────────┘ │ │ │
│ │ └──────────────────────────────────────────────────┘ │ │
│ │ ┌──────────────────────────────────────────────────┐ │ │
│ │ │ All Feedback (3)                                 │ │ │
│ │ │ ┌──────────────────────────────────────────────┐ │ │ │
│ │ │ │ FeedbackList                                 │ │ │ │
│ │ │ │ • User feedback items                        │ │ │ │
│ │ │ │ • Comments & corrections                     │ │ │ │
│ │ │ │ • Status & timestamps                        │ │ │ │
│ │ │ └──────────────────────────────────────────────┘ │ │ │
│ │ └──────────────────────────────────────────────────┘ │ │
│ └──────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────┘
```

### 3. FeedbackButton Component
```
┌────────────────────────────────────┐
│ FeedbackButton                     │
│                                    │
│  ┌──────────┐  ┌──────────┐      │
│  │    👍   │  │    👎   │      │
│  │ Correct  │  │Incorrect │      │
│  └──────────┘  └──────────┘      │
│                                    │
│  States:                           │
│  • Default (outline)               │
│  • Active (filled green/red)       │
│  • Loading (spinner)               │
│  • Disabled (grayed out)           │
└────────────────────────────────────┘
```

### 4. FeedbackForm Component
```
┌─────────────────────────────────────────────┐
│ FeedbackForm                                │
│                                             │
│ Feedback Type:                              │
│ ┌─────────────────────────────────────────┐ │
│ │ ○ Correct Detection                     │ │
│ │ ○ False Positive                        │ │
│ │ ○ False Negative                        │ │
│ │ ○ Incorrect Classification              │ │
│ │ ○ Incorrect Risk Score                  │ │
│ │ ○ Incorrect AI Provider                 │ │
│ └─────────────────────────────────────────┘ │
│                                             │
│ Additional Comments (Optional):             │
│ ┌─────────────────────────────────────────┐ │
│ │ [textarea]                              │ │
│ └─────────────────────────────────────────┘ │
│                                             │
│ ✏️ Add Suggested Corrections               │
│ ┌─────────────────────────────────────────┐ │
│ │ Correct Automation Type: [input]        │ │
│ │ Correct AI Provider: [input]            │ │
│ │ Correct Risk Level: [select]            │ │
│ │ Correct Risk Score: [input 0-100]       │ │
│ │ Additional Notes: [textarea]            │ │
│ └─────────────────────────────────────────┘ │
│                                             │
│           [Cancel]  [Submit Feedback]       │
└─────────────────────────────────────────────┘
```

### 5. FeedbackList Component
```
┌─────────────────────────────────────────────┐
│ FeedbackList                                │
│                                             │
│ ┌─────────────────────────────────────────┐ │
│ │ 👍 correct_detection  • 2 days ago      │ │
│ │ "This detection is accurate!"           │ │
│ │ user@example.com • pending              │ │
│ └─────────────────────────────────────────┘ │
│                                             │
│ ┌─────────────────────────────────────────┐ │
│ │ 👎 false_positive  • 5 days ago         │ │
│ │ "This is not an automation"             │ │
│ │ Suggested: Type = workflow              │ │
│ │ admin@example.com • acknowledged        │ │
│ └─────────────────────────────────────────┘ │
│                                             │
│ ┌─────────────────────────────────────────┐ │
│ │ 👎 incorrect_ai_provider  • 1 week ago  │ │
│ │ "Should be OpenAI, not Claude"          │ │
│ │ Suggested: Provider = OpenAI            │ │
│ │ user2@example.com • resolved            │ │
│ └─────────────────────────────────────────┘ │
└─────────────────────────────────────────────┘
```

## Data Flow

### 1. Feedback Submission Flow
```
User Action                Component              API Call              Backend
    │                          │                      │                    │
    ├─ Click 👍              →│                      │                    │
    │                          ├─ Open Form          │                    │
    │                          │                      │                    │
    ├─ Fill Form              →│                      │                    │
    │                          │                      │                    │
    ├─ Click Submit           →│                      │                    │
    │                          ├─ createFeedback()  →│                    │
    │                          │                      ├─ POST /feedback  →│
    │                          │                      │                    ├─ Validate
    │                          │                      │                    ├─ Store
    │                          │                      │                    ├─ Generate ML
    │                          │                      │◄─ Response        │
    │                          │◄─ Success            │                    │
    ├─ Toast: "Success!"      ←│                      │                    │
    ├─ Update UI              ←│                      │                    │
    └─────────────────────────────────────────────────────────────────────┘
```

### 2. Feedback Fetch Flow
```
Component Mount            Component              API Call              Backend
    │                          │                      │                    │
    ├─ useEffect()            →│                      │                    │
    │                          ├─ getFeedback()      →│                    │
    │                          │                      ├─ GET /feedback   →│
    │                          │                      │                    ├─ Query DB
    │                          │                      │                    ├─ Filter
    │                          │                      │◄─ Response        │
    │                          │◄─ Feedback[]         │                    │
    ├─ Display feedback       ←│                      │                    │
    └─────────────────────────────────────────────────────────────────────┘
```

## State Management

### AutomationFeedback Component State
```typescript
{
  isExpanded: boolean,          // Form visibility
  isLoading: boolean,           // API call in progress
  currentFeedback: AutomationFeedback | null,  // Existing feedback
  pendingSentiment: FeedbackSentiment | null   // Selected sentiment
}
```

### FeedbackForm Component State
```typescript
{
  feedbackType: FeedbackType,   // Selected type
  comment: string,              // User comment
  showCorrections: boolean,     // Corrections section visibility
  // Correction fields
  automationType: string,
  aiProvider: string,
  riskLevel: string,
  riskScore: string,
  correctionNotes: string
}
```

### AutomationDetailsModal State (Extended)
```typescript
{
  // Existing state...
  feedbackList: AutomationFeedback[],  // [NEW] All feedback
  isFeedbackLoading: boolean           // [NEW] Loading state
}
```

## API Integration

### Feedback API Client
```typescript
feedbackApi = {
  // Core operations
  createFeedback(input: CreateFeedbackInput): Promise<ApiResponse>
  getFeedback(id: string): Promise<ApiResponse>
  getFeedbackList(filters?: FeedbackFilters): Promise<ApiResponse>

  // Automation-specific
  getFeedbackByAutomation(automationId: string): Promise<ApiResponse>

  // Recent & stats
  getRecentFeedback(orgId: string, limit?: number): Promise<ApiResponse>
  getStatistics(orgId: string): Promise<ApiResponse>
  getTrends(orgId: string, days?: number): Promise<ApiResponse>

  // Management
  updateFeedback(id: string, input: UpdateFeedbackInput): Promise<ApiResponse>
  acknowledgeFeedback(id: string): Promise<ApiResponse>
  resolveFeedback(id: string, resolution: FeedbackResolution): Promise<ApiResponse>

  // ML training
  exportMLTrainingBatch(orgId?: string, limit?: number): Promise<ApiResponse>
}
```

## User Interaction Flows

### Flow 1: Quick Positive Feedback
```
1. User sees automation card
2. Clicks 👍 button
3. Form opens with "Correct Detection" pre-selected
4. Optionally adds comment
5. Clicks "Submit Feedback"
6. Toast shows success
7. Button shows active state (green)
```

### Flow 2: Detailed Negative Feedback
```
1. User sees automation card
2. Clicks 👎 button
3. Form opens with feedback type options
4. Selects "Incorrect AI Provider"
5. Adds comment: "This is OpenAI, not Claude"
6. Clicks "Add Suggested Corrections"
7. Enters correct provider: "OpenAI"
8. Clicks "Submit Feedback"
9. Toast shows success
10. Button shows active state (red)
```

### Flow 3: View Feedback History
```
1. User clicks "View Details" on automation card
2. Modal opens
3. User clicks "Feedback" tab
4. Sees own feedback at top
5. Sees all team feedback below
6. Can click "Edit" to modify own feedback
```

## Responsive Behavior

### Desktop View
- Full width forms
- Side-by-side buttons
- Detailed feedback cards
- Modal overlay for expanded forms

### Tablet View
- Slightly compressed layout
- Stacked buttons for narrow screens
- Scrollable feedback list
- Same modal behavior

### Mobile View
- Compact feedback buttons
- Full-screen modal for feedback form
- Simplified feedback cards
- Touch-optimized button sizes

## Accessibility Features

### Keyboard Navigation
- Tab through all interactive elements
- Enter to submit forms
- Escape to close modals
- Arrow keys for radio buttons

### Screen Reader Support
- Descriptive ARIA labels
- Form field labels
- Button descriptions
- Status announcements

### Visual Indicators
- Focus outlines
- Active states
- Loading spinners
- Error messages

---

**Last Updated**: October 11, 2025
**Version**: 1.0
**Status**: Production Ready
