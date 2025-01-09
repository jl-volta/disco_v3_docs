# Database Schema

## Overview
Integration with existing database and new tables for task management system.

## Existing Tables

### companies
```sql
-- Using existing companies table
-- Relevant fields:
--   id uuid
--   business_name text
--   assigned_coach uuid
--   other fields...
```

### people
```sql
-- Using existing people table
-- Relevant fields:
--   id uuid
--   user_id uuid (links to auth.users)
--   linkedin_id text
--   first_name text
--   last_name text
--   email text
```

### people_companies
```sql
-- Using existing junction table
-- Relevant fields:
--   person_id uuid
--   company_id uuid
--   role_id uuid
```

## New Tables

### objectives
```sql
create table objectives (
    id uuid default uuid_generate_v4() primary key,
    name text not null,
    order_index integer not null,
    description text,
    constraint unique_order unique (order_index)
);

-- Initial data
insert into objectives (name, order_index, description) values
    ('Value Baseline', 1, 'Initial value validation phase'),
    ('First Paying Customer', 2, 'Acquisition of first customer'),
    ('X Paying Customers', 3, 'Growing customer base'),
    ('10X Paying Customers', 4, 'Scaling customer acquisition'),
    ('1M+ ARR', 5, 'Reaching significant revenue milestone');
```

### objectives_companies
```sql
create table objectives_companies (
    id uuid default uuid_generate_v4() primary key,
    company_id uuid references companies(id) on delete cascade,
    objective_id uuid references objectives(id) on delete cascade,
    achieved_at timestamp with time zone default timezone('utc'::text, now()) not null,
    updated_by uuid references people(id) not null,
    constraint unique_company_objective unique (company_id, objective_id)
);
```

### tasks
```sql
create table tasks (
    id uuid default uuid_generate_v4() primary key,
    company_id uuid references companies(id) on delete cascade,
    objective_id uuid references objectives(id),
    title text not null,
    description text,
    assignee_id uuid references people(id),
    due_date timestamp with time zone,
    status text not null check (status in ('not_started', 'in_progress', 'completed', 'failed')),
    failure_reason text,
    completed_at timestamp with time zone,
    created_at timestamp with time zone default timezone('utc'::text, now()) not null,
    created_by uuid references people(id) not null
);
```

### task_activity_log
```sql
create table task_activity_log (
    id uuid default uuid_generate_v4() primary key,
    task_id uuid references tasks(id) on delete cascade,
    person_id uuid references people(id) not null,
    action_type text not null check (action_type in ('progress_update', 'status_change', 'comment_added', 'attachment_added')),
    content text,
    old_status text,
    new_status text,
    created_at timestamp with time zone default timezone('utc'::text, now()) not null
);
```

### task_comments
```sql
create table task_comments (
    id uuid default uuid_generate_v4() primary key,
    task_id uuid references tasks(id) on delete cascade,
    person_id uuid references people(id) not null,
    content text not null,
    created_at timestamp with time zone default timezone('utc'::text, now()) not null
);
```

### task_attachments
```sql
create table task_attachments (
    id uuid default uuid_generate_v4() primary key,
    task_id uuid references tasks(id) on delete cascade,
    file_path text not null,
    file_name text not null,
    file_type text not null,
    file_size integer not null,
    uploaded_by uuid references people(id) not null,
    created_at timestamp with time zone default timezone('utc'::text, now()) not null
);

-- File upload constraints
-- Enforced in application logic
MAX_FILE_SIZE = 10MB
ALLOWED_TYPES = [
  'application/pdf',
  'image/jpeg',
  'image/png',
  'image/gif',
  'application/msword',
  'application/vnd.openxmlformats-officedocument.wordprocessingml.document',
  'application/vnd.ms-excel',
  'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet'
]
```

## Indexes
```sql
-- Task related indexes
create index idx_tasks_company_id on tasks(company_id);
create index idx_tasks_assignee_id on tasks(assignee_id);
create index idx_tasks_objective_id on tasks(objective_id);
create index idx_tasks_status on tasks(status);

-- Activity log indexes
create index idx_task_activity_log_task_id on task_activity_log(task_id);
create index idx_task_activity_log_person_id on task_activity_log(person_id);

-- Comments and attachments indexes
create index idx_task_comments_task_id on task_comments(task_id);
create index idx_task_attachments_task_id on task_attachments(task_id);

-- Objectives indexes
create index idx_objectives_companies_company_id on objectives_companies(company_id);
create index idx_objectives_companies_objective_id on objectives_companies(objective_id);
``` 