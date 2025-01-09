# Company Management

## Overview
System for managing companies and their progress through defined business objectives, integrated with the existing company database.

## Core Functionality

### Data Structure
```typescript
interface Objective {
  id: string;
  name: string;
  order_index: number;
  description: string;
}

interface ObjectiveAchievement {
  company_id: string;
  objective_id: string;
  achieved_at: Date;
  updated_by: string; // person_id
}

interface CompanyView {
  id: string;
  business_name: string;
  current_objective: Objective;
  objectives_achieved: ObjectiveAchievement[];
  task_metrics: {
    total_active: number;
    completed_this_objective: number;
    overdue: number;
  };
  assigned_coach: {
    person_id: string;
    first_name: string;
    last_name: string;
  };
}
```

### Objectives
1. Value Baseline
2. First Paying Customer
3. X Paying Customers
4. 10X Paying Customers
5. 1M+ ARR

### Company Operations
- View assigned companies
- Track objective achievements (coach only)
- Manage tasks per objective
  - Coaches: Full task management
  - Team members: Progress updates and comments
- View company metrics

## User Interfaces

### Coach Dashboard
- List of assigned companies
  - Company name and basic info
  - Current objective
  - Task activity summary
  - Quick actions
    - Task status changes
- Filters
  - By objective
  - By activity status
  - By task status

### Company Detail View
#### Coach View
- Objective Information
  - Current objective
  - Achievement timeline
  - Task completion metrics
- Task Management
  - Create/Edit tasks
  - Review progress updates
  - Update task statuses
  - View task history
  - Track completion
- Company Information
  - Team member list
  - Basic company details
  - Activity history

#### Team Member View
- Objective Information
  - Current objective (read-only)
  - Achievement timeline
  - Task completion metrics
- Task Management
  - View assigned tasks
  - Submit progress updates
  - Add comments
  - Upload attachments
- Company Information
  - Team member list
  - Basic company details
  - Activity history

## Success Criteria
- [ ] Integration with existing company database
- [ ] Display of company objective status
- [ ] Role-based task management
- [ ] Progress update logging
- [ ] Objective progress tracking
- [ ] Company metrics dashboard
- [ ] Coach assignment validation

## Database Integration
### Existing Tables
```sql
-- Existing tables structure for reference
CREATE TABLE companies (
    id UUID PRIMARY KEY,
    business_name VARCHAR(255) NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT NOW()
);

CREATE TABLE people (
    id UUID PRIMARY KEY,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT NOW()
);

CREATE TABLE people_companies (
    person_id UUID REFERENCES people(id),
    company_id UUID REFERENCES companies(id),
    role VARCHAR(50) NOT NULL,
    PRIMARY KEY (person_id, company_id)
);
```

### New Tables
```sql
-- Create enum for task status
CREATE TYPE task_status AS ENUM ('not_started', 'in_progress', 'completed', 'failed');

-- Create enum for activity action types
CREATE TYPE activity_action_type AS ENUM (
    'task_created',
    'task_assigned',
    'due_date_changed',
    'title_changed',
    'description_changed',
    'progress_update',
    'progress_update_deleted',
    'status_change',
    'comment_added',
    'comment_edited',
    'comment_deleted',
    'attachment_added',
    'attachment_deleted'
);

-- Create enum for allowed file types
CREATE TYPE allowed_file_type AS ENUM (
    'jpg', 'jpeg', 'png', 'gif',
    'pdf', 'doc', 'docx', 'xls', 'xlsx',
    'txt', 'md', 'zip'
);

CREATE TABLE objectives (
    id UUID PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    order_index INTEGER NOT NULL,
    description TEXT NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT NOW()
);

CREATE TABLE objectives_companies (
    company_id UUID REFERENCES companies(id),
    objective_id UUID REFERENCES objectives(id),
    achieved_at TIMESTAMP WITH TIME ZONE,
    updated_by UUID REFERENCES people(id),
    PRIMARY KEY (company_id, objective_id)
);

CREATE TABLE tasks (
    id UUID PRIMARY KEY,
    company_id UUID REFERENCES companies(id),
    objective_id UUID REFERENCES objectives(id),
    title VARCHAR(255) NOT NULL,
    description TEXT NOT NULL,
    assignee_id UUID REFERENCES people(id),
    due_date TIMESTAMP WITH TIME ZONE NOT NULL,
    status task_status NOT NULL DEFAULT 'not_started',
    failure_reason TEXT,
    failed_at TIMESTAMP WITH TIME ZONE,
    completed_at TIMESTAMP WITH TIME ZONE,
    created_at TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT NOW(),
    created_by UUID REFERENCES people(id)
);

CREATE TABLE progress_updates (
    id UUID PRIMARY KEY,
    task_id UUID REFERENCES tasks(id),
    submitted_by UUID REFERENCES people(id),
    content TEXT NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT NOW(),
    deleted_at TIMESTAMP WITH TIME ZONE
);

CREATE TABLE task_comments (
    id UUID PRIMARY KEY,
    task_id UUID REFERENCES tasks(id),
    person_id UUID REFERENCES people(id),
    content TEXT NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT NOW(),
    deleted_at TIMESTAMP WITH TIME ZONE
);

CREATE TABLE task_attachments (
    id UUID PRIMARY KEY,
    task_id UUID REFERENCES tasks(id),
    file_name VARCHAR(255) NOT NULL,
    file_size INTEGER NOT NULL,
    file_type allowed_file_type NOT NULL,
    upload_url TEXT NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT NOW(),
    created_by UUID REFERENCES people(id),
    deleted_at TIMESTAMP WITH TIME ZONE
);

CREATE TABLE task_activity_log (
    id UUID PRIMARY KEY,
    task_id UUID REFERENCES tasks(id),
    person_id UUID REFERENCES people(id),
    action_type activity_action_type NOT NULL,
    content TEXT,
    old_status task_status,
    new_status task_status,
    old_assignee UUID REFERENCES people(id),
    new_assignee UUID REFERENCES people(id),
    old_due_date TIMESTAMP WITH TIME ZONE,
    new_due_date TIMESTAMP WITH TIME ZONE,
    old_title VARCHAR(255),
    new_title VARCHAR(255),
    old_description TEXT,
    new_description TEXT,
    old_comment TEXT,
    created_at TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT NOW()
);

-- Indexes for performance
CREATE INDEX idx_tasks_company ON tasks(company_id);
CREATE INDEX idx_tasks_objective ON tasks(objective_id);
CREATE INDEX idx_tasks_assignee ON tasks(assignee_id);
CREATE INDEX idx_progress_updates_task ON progress_updates(task_id);
CREATE INDEX idx_progress_updates_deleted ON progress_updates(deleted_at);
CREATE INDEX idx_task_comments_task ON task_comments(task_id);
CREATE INDEX idx_task_comments_deleted ON task_comments(deleted_at);
CREATE INDEX idx_task_attachments_task ON task_attachments(task_id);
CREATE INDEX idx_task_attachments_deleted ON task_attachments(deleted_at);
CREATE INDEX idx_task_activity_task ON task_activity_log(task_id);
CREATE INDEX idx_task_activity_type ON task_activity_log(action_type);
CREATE INDEX idx_task_activity_date ON task_activity_log(created_at);
```

## Access Control
### Role Definitions
```typescript
enum UserRole {
  COACH = 'coach',
  COMPANY_MEMBER = 'company_member'
}

interface AccessControl {
  role: UserRole;
  permissions: Permission[];
}

interface Permission {
  resource: string;
  actions: string[];
}
```

### Permission Matrix
#### Coach
- Tasks
  - Create, Read, Update, Delete
  - Assign to company members
  - Change status
- Progress Updates
  - Read
- Comments
  - Create, Read
- Attachments
  - Read, Delete
- Company Information
  - Read
- Objectives
  - Read, Update achievement status

#### Company Member
- Tasks
  - Read assigned tasks
  - Submit progress updates
- Progress Updates
  - Create, Read own updates
- Comments
  - Create, Read
- Attachments
  - Create, Read own attachments
- Company Information
  - Read

### Access Rules
1. Coaches can only access companies they are assigned to
2. Company members can only access their own company
3. Task status changes restricted to coaches
4. File uploads must pass virus scan
5. All actions logged in activity log
6. Sensitive operations require re-authentication
7. Rate limiting applied to API endpoints 