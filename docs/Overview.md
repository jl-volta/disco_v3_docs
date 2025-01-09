---
layout: default
title: Overview
nav_order: 1
---

# Team Coaching Task Manager - Project Overview

## Directory Structure
```
docs/
├── Tasks/
│   ├── MVP Overview.md           # Current phase and priorities
│   ├── Auth Tasks.md            # LinkedIn OAuth + People integration
│   ├── Company Tasks.md         # Company management features
│   └── Task System Tasks.md     # Task management features
├── Features/
│   ├── Core/
│   │   ├── Authentication.md    # LinkedIn OAuth + People integration
│   │   ├── Company Management.md # Company structure and objectives
│   │   └── Task System.md       # Task management and tracking
│   ├── UI/
│   │   ├── Layout.md           # App layout and navigation
│   │   ├── Company Views.md    # Company management interfaces
│   │   └── Task Views.md       # Task management interfaces
│   └── Backend/
│       ├── Database Schema.md   # Supabase schema integration
│       └── Security Rules.md    # RLS policies and security
```

## Project Goals
- Create a task management system for coaching multiple companies
- Track company progress through business objectives (Value Baseline → 1M+ ARR)
- Enable coaches to manage and monitor multiple companies (1-25) simultaneously
- Integrate with existing company/people database
- Provide company members with clear task visibility and progress tracking

## Core Features

### Company Management
- Company progress tracking through objectives
- Member management via existing database
- Objective achievement tracking
- Progress monitoring
- Data integration

### Task System
- Task creation and assignment
- Status tracking
- File attachments
- Comments and discussions
- Objective association

### Authentication
- LinkedIn OAuth integration
- People table integration
- Company-based permissions
- Role-based access control

## Development Phases

### Phase 1: MVP
- Basic company integration
- Core task functionality
- LinkedIn OAuth + People integration
- Basic UI/UX with shadcn/ui

### Phase 2: Enhanced Features
- Comments and discussions
- File attachments
- Advanced tracking
- Enhanced UI/UX with custom components

### Phase 3: Integration & Optimization
- Deep integration with existing systems
- Performance optimization
- Advanced analytics
- Enhanced security 