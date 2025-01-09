# Team Management Interfaces

## Overview
UI components and views for team management functionality.

## Views

### Team Dashboard
- Team overview stats
- Current objective
- Recent activity
- Member list
- Task summary

### Team Creation
- Team details form
- Member invitation
- Initial objective setting
- Role assignment

### Team Settings
- Team profile
- Member management
- Objective updates
- Integration settings

## Components

### Team Card
```typescript
interface TeamCardProps {
  team: Team;
  onEdit?: () => void;
  onDelete?: () => void;
}
```

### Member List
```typescript
interface MemberListProps {
  members: TeamMember[];
  onAdd?: () => void;
  onRemove?: (id: string) => void;
}
```

### Progress Tracker
```typescript
interface ProgressTrackerProps {
  currentObjective: ObjectiveStage;
  progress: number;
}
```

## Success Criteria
- [ ] Responsive layouts
- [ ] Intuitive navigation
- [ ] Clear data presentation
- [ ] Efficient workflows 