# Task System

## Overview
Task management system for tracking company activities within each business objective phase.

## Core Functionality

### Task Structure
```typescript
interface Task {
  id: string;
  company_id: string;
  objective_id: string;
  title: string;
  description: string;
  assignee: {
    person_id: string;
    first_name: string;
    last_name: string;
  };
  due_date: Date;
  status: TaskStatus;
  failure_reason?: string;
  failed_at?: Date;
  completed_at?: Date;
  created_at: Date;
  created_by: string; // coach's person_id
}

interface ProgressUpdate {
  id: string;
  task_id: string;
  submitted_by: string; // person_id
  content: string;
  attachments: Attachment[];
  created_at: Date;
}

interface Attachment {
  id: string;
  file_name: string;
  file_size: number; // in bytes
  file_type: string;
  upload_url: string;
  created_at: Date;
  created_by: string; // person_id
}

interface TaskTemplate {
  id: string;
  objective_id: string;
  title: string;
  description: string;
  created_at: Date;
  last_used_at?: Date;
  usage_count: number;
}

enum TaskStatus {
  NOT_STARTED = 'not_started',
  IN_PROGRESS = 'in_progress',
  COMPLETED = 'completed',
  FAILED = 'failed'
}

interface TaskActivity {
  id: string;
  task_id: string;
  person_id: string;
  action_type: 'task_created' | 'task_assigned' | 'due_date_changed' | 'title_changed' | 'description_changed' | 'progress_update' | 'status_change' | 'comment_added' | 'comment_edited' | 'comment_deleted' | 'attachment_added';
  content?: string;
  old_status?: string;
  new_status?: string;
  old_assignee?: string;
  new_assignee?: string;
  old_due_date?: Date;
  new_due_date?: Date;
  old_title?: string;
  new_title?: string;
  old_description?: string;
  new_description?: string;
  old_comment?: string;
  created_at: Date;
}
```

### File Upload Specifications
- Maximum file size: 10MB per file
- Total storage per task: 50MB
- Allowed file types:
  - Images: .jpg, .jpeg, .png, .gif
  - Documents: .pdf, .doc, .docx, .xls, .xlsx
  - Text: .txt, .md
  - Archives: .zip (max 20MB)

### Task Operations

#### Coach Operations
- Create tasks for specific objectives
  - Due date required at creation
  - Defaults to 14 days from creation date
- Assign tasks to company members
- Update task status (exclusive to coaches)
  - COMPLETED: No additional input required
  - FAILED: Must provide failure reason
- Set due dates
- Mark tasks as complete/failed
- Review progress updates
- Add comments
  - Can edit own comments (tracked in activity log)
  - Can delete own comments (tracked in activity log)

#### Company Member Operations
- View assigned tasks
- Log progress updates
- Add comments
  - Can edit own comments (tracked in activity log)
  - Can delete own comments (tracked in activity log)
- Add attachments

### Progress Tracking
- Activity log per task
- Progress updates from team members
- Status changes (by coaches only)
- Comment thread
- File attachments

### Task Organization
- Grouped by status:
  1. Active (NOT_STARTED, IN_PROGRESS)
  2. Completed
  3. Failed
- All tasks visible in tabs (no pagination)
- Tasks within each tab sorted by due date

### Task Templates
- Predefined set of task templates in database
- Templates associated with specific objectives
- Shared across all coaches
- No UI for template creation in MVP
- Templates serve as quick-start options
- Template usage tracking for optimization

## User Interfaces

### Coach View
- Task creation and management
  - Associate with objectives
  - Assign to team members
- Progress review dashboard
  - Filter by objective
  - Filter by status
- Activity log review
- Task status management
- File attachment review

### Company Member View
- Task list and details
  - Grouped by objective
  - Sorted by due date
- Progress update submission
- Comment addition
- File upload
- Activity log view

### Progress Update Workflow
1. Company member submits update
   - Required: content
   - Optional: attachments
2. Update appears in task activity log
3. Coach can review updates during status assessment

## Success Criteria
- [ ] Coaches can create and manage tasks
- [ ] Tasks properly linked to objectives
- [ ] Company members can log progress
- [ ] Only coaches can update task status
- [ ] Activity tracking works
- [ ] File attachments work
- [ ] Comments system works
- [ ] Clear activity history 