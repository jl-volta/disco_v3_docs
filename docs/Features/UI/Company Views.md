# Company Management Interfaces

## Overview
UI components and views for company management functionality.

## Views

### Company Dashboard
- Company overview stats
  - Current objective
  - Active tasks count
  - Recent activity
  - Task completion rate
- Quick filters
  - By objective
  - By task status
  - By activity

### Company Detail View
Tabs:
1. Overview
   - Company details
   - Key contacts
   - Basic info
2. Tasks & Progress
   - Current objective banner
   - Objective history timeline
   - Task management interface
   - Activity feed

### Components

### Company Card
```typescript
interface CompanyCardProps {
  company: {
    id: string;
    business_name: string;
    current_objective: {
      name: string;
      order_index: number;
    };
    task_metrics: {
      total_active: number;
      completed: number;
    };
  };
  onSelect?: () => void;
}
```

### Objective Timeline
```typescript
interface ObjectiveTimelineProps {
  objectives: {
    name: string;
    achieved_at?: Date;
    order_index: number;
  }[];
  current_objective_id: string;
}
```

### Task List
```typescript
interface TaskListProps {
  company_id: string;
  objective_id?: string;
  onTaskSelect?: (task: Task) => void;
  onStatusChange?: (task_id: string, status: TaskStatus) => void;
}
```

## Success Criteria
- [ ] Responsive layouts
- [ ] Clear objective progression
- [ ] Intuitive task management
- [ ] Efficient activity tracking 