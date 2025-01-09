# MVP Overview

## Sprint 1: Core Task Management

### Authentication & Setup
- [ ] LinkedIn OAuth integration with existing people table
- [ ] Company data integration
- [ ] Basic layout with shadcn/ui
- [ ] Protected routes setup

### Coach Dashboard
- [ ] Companies list view with current objectives
- [ ] Company filtering by objective/status
- [ ] Task metrics per company
- [ ] Navigation to company details

### Task Management (Core)
- [ ] Task creation form with objective association
- [ ] Task list view per company
- [ ] Task status management by coaches
- [ ] Activity logging system

## Sprint 2: Progress Tracking

### Progress System
- [ ] Activity log implementation
- [ ] Progress update form for company members
- [ ] Progress review view for coaches
- [ ] Task history view

### Comments System
- [ ] Comment creation
- [ ] Comment thread view
- [ ] Comment notifications (if time permits)

## Sprint 3: File Management

### File Attachments
- [ ] Supabase storage setup
- [ ] File upload interface
- [ ] File preview/download
- [ ] Attachment management

## Dependencies
- Supabase project setup
- LinkedIn OAuth credentials
- Access to existing database (companies, people, people_companies)

## Success Metrics
- Coaches can fully manage tasks per objective
- Company members can log progress
- Clear activity tracking
- Smooth file management 