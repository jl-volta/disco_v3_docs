# Company Management

## Overview
System for managing companies (teams) and their progress through business milestones, integrated with existing company database.

## Core Functionality

### Company Structure
```typescript
interface Company {
  id: string;
  business_name: string;
  market_milestone_reached: MarketMilestone;
  traction_level: TractionLevel;
  assigned_coach: string; // UUID of advisor
  members: CompanyMember[];
}

enum MarketMilestone {
  VALUE_BASELINE = 'value_baseline',
  FIRST_CUSTOMER = 'first_customer',
  X_CUSTOMERS = 'x_customers',
  TEN_X_CUSTOMERS = 'ten_x_customers',
  ONE_M_ARR = 'one_m_arr'
}

interface CompanyMember {
  person_id: string;
  company_id: string;
  role_id: string;
  person: {
    first_name: string;
    last_name: string;
    email: string;
    linkedin_id?: string;
  };
}
```

### Company Operations
- View and filter companies
- Track milestone progression
- Manage member assignments
- Monitor progress
- Export company data

### Milestone Management
- Progress tracking
- Success verification
- Historical tracking
- Traction level monitoring

## User Interfaces

### Coach View
- Company overview dashboard
- Progress tracking
- Member management
- Multi-company monitoring
- Task assignment

### Founder View
- Company details
- Current milestone
- Task overview
- Team composition

## Success Criteria
- [ ] Company data integration
- [ ] Member management
- [ ] Milestone tracking
- [ ] Progress visualization
- [ ] Task management integration 