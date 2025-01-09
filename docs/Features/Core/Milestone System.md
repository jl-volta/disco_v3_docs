# Milestone System

## Overview
System for tracking company progress through market milestones with binary success criteria and activity metrics.

## Core Functionality

### Milestone Structure
```typescript
interface MilestoneStatus {
  company_id: string;
  milestone: MarketMilestone;
  achieved: boolean;
  started_at: Date;
  achieved_at?: Date;
  last_updated: Date;
  updated_by: string; // coach's person_id
  evidence?: MilestoneEvidence[];
}

interface MilestoneEvidence {
  id: string;
  milestone_status_id: string;
  evidence_type: 'document' | 'metric' | 'link';
  title: string;
  description: string;
  url?: string;
  attachment_id?: string;
  created_at: Date;
  created_by: string; // coach's person_id
}

interface MilestoneMetrics {
  days_in_milestone: number;
  total_tasks: number;
  completed_tasks: number;
  active_tasks: number;
  avg_task_completion_time: number;
  progress_update_count: number;
  evidence_count: number;
}

enum MarketMilestone {
  VALUE_BASELINE = 'value_baseline',
  FIRST_CUSTOMER = 'first_customer',
  X_CUSTOMERS = 'x_customers',
  TEN_X_CUSTOMERS = 'ten_x_customers',
  ONE_M_ARR = 'one_m_arr'
}
```

### Milestone Criteria

#### Value Baseline
- Clear value proposition documented
- Target market identified
- Initial pricing model defined
- MVP features outlined

#### First Customer
- Signed contract/agreement
- Payment received
- Product/service delivered
- Customer feedback documented

#### X Customers
- Minimum 5 paying customers
- Recurring revenue established
- Customer retention > 80%
- Average deal size tracked

#### 10X Customers
- Minimum 50 paying customers
- Monthly recurring revenue growth
- Customer acquisition cost defined
- Sales process documented

#### 1M+ ARR
- $1M annual recurring revenue
- Sustainable growth metrics
- Positive unit economics
- Market expansion plan

### Milestone Operations
- Only coaches can toggle milestone achievement
- Track time spent in each milestone
- Calculate task completion metrics
  - Based on coach-approved task completions
  - Includes progress update statistics
- View milestone history
- Monitor task approval rates
- Evidence required for milestone completion
- Automatic notification of milestone changes

### Progress Tracking
- Binary achievement status (achieved/not achieved)
- Time-based metrics
  - Days in current milestone
  - Date started
  - Date achieved
  - Average time in milestone (across companies)
- Task-based metrics
  - Number of completed tasks (coach-approved)
  - Number of active tasks
  - Total tasks created
  - Progress update approval rate
  - Average time to task completion
- Evidence tracking
  - Required documents
  - Metric achievements
  - External links/references

## User Interfaces

### Milestone Toggle
```typescript
interface MilestoneToggle {
  company_id: string;
  milestone: MarketMilestone;
  achieved: boolean;
  coach_id: string;
  reason?: string;
}
```

### Milestone Dashboard
- Current milestone status
- Days in current milestone
- Task completion statistics
  - Approved vs pending updates
  - Task completion trends
- Progress update metrics
- Historical milestone data

### Company View
- Current milestone status (read-only)
- Progress metrics
- Task completion rates
- Historical milestone achievements

## Success Criteria
- [ ] Only coaches can toggle milestone achievement
- [ ] System tracks time in milestone
- [ ] Task statistics reflect approved completions
- [ ] Progress update metrics are tracked
- [ ] Milestone history is maintained
- [ ] Clear milestone status display
- [ ] Company members have appropriate read-only access 