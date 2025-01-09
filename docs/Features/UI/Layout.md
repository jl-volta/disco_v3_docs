# Application Layout

## Overview
Core layout structure using shadcn/ui components and Tailwind CSS, matching existing application design.

## Layout Structure

### Sidebar Navigation
- Fixed position left sidebar
- Dark theme
- Menu items:
  - Dashboard (overview)
  - Companies
  - Tasks
  - Reports (if needed)

### Main Content Area
```typescript
interface MainLayoutProps {
  children: React.ReactNode;
  header: {
    title: string;
    subtitle?: string;
  };
  stats?: {
    label: string;
    value: string | number;
    icon?: React.ReactNode;
  }[];
}
```

### Page Structure
- Header section
  - Page title
  - Subtitle/description
- Stats cards row (when applicable)
  - Key metrics in card format
- Main content area
  - Dynamic content per route
  - Card-based layouts
- Action buttons
  - Consistent positioning
  - Clear hierarchy

## Component Patterns

### Stats Card
```typescript
interface StatsCardProps {
  label: string;
  value: string | number;
  icon?: React.ReactNode;
}
```

### Content Card
```typescript
interface ContentCardProps {
  title?: string;
  children: React.ReactNode;
  footer?: React.ReactNode;
  actions?: React.ReactNode;
}
```

### Action Buttons
- Primary actions: Blue
- Secondary actions: Gray
- Destructive actions: Red
- Clear labeling
- Consistent positioning

## Theme
- Dark background
- Card-based content
- Consistent spacing
- Clear typography hierarchy
- Accent colors for actions/status 