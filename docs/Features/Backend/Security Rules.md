# Security Rules

## Overview
Supabase Row Level Security (RLS) policies and access control.

## Authentication Rules

### User Authentication
- LinkedIn OAuth required
- Session management via Supabase
- Token refresh handling

## Row Level Security Policies

### Teams Table
```sql
-- Allow coaches to read all teams
create policy "Coaches can read all teams"
on teams for select
to authenticated
using (
  exists (
    select 1 from team_members
    where team_members.user_id = auth.uid()
    and team_members.role = 'coach'
  )
);

-- Allow founders to read their teams
create policy "Founders can read their teams"
on teams for select
to authenticated
using (
  exists (
    select 1 from team_members
    where team_members.team_id = teams.id
    and team_members.user_id = auth.uid()
  )
);

-- Only coaches can create/update teams
create policy "Coaches can create teams"
on teams for insert
to authenticated
using (
  exists (
    select 1 from team_members
    where team_members.user_id = auth.uid()
    and team_members.role = 'coach'
  )
);
```

### Tasks Table
```sql
-- Team members can read their team's tasks
create policy "Team members can read their tasks"
on tasks for select
to authenticated
using (
  exists (
    select 1 from team_members
    where team_members.team_id = tasks.team_id
    and team_members.user_id = auth.uid()
  )
);

-- Assignees can update their tasks
create policy "Assignees can update their tasks"
on tasks for update
to authenticated
using (
  assignee_id = auth.uid()
);
```

## Access Control Matrix

### Coach Permissions
- Read all team data
- Create/update teams
- Manage team members
- Create/update tasks
- View all tasks

### Founder Permissions
- Read own team data
- Update assigned tasks
- Create task comments
- Upload attachments

## Security Best Practices
- Validate all user input
- Sanitize file uploads
- Implement rate limiting
- Monitor auth attempts
- Regular security audits 

## Credential Management

### Environment Variables
```env
# .env
NEXT_PUBLIC_SUPABASE_URL=your-project-url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
```

### Production Security
- All credentials must be stored in environment variables
- Never commit .env files to version control
- Rotate keys if accidentally exposed
- Use separate development and production keys 