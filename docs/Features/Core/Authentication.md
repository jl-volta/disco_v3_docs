# Authentication System

## Overview
Authentication system using LinkedIn OAuth via Supabase Auth, integrated with existing people and companies database.

## User Flows

### Login Flow
1. User clicks "Sign in with LinkedIn"
2. LinkedIn OAuth modal opens
3. User authenticates with LinkedIn
4. On successful auth:
   - Check if LinkedIn ID exists in people table
   - If new user, create people record
   - Link Supabase auth.user to people record
5. Load user's company associations
6. Redirect to appropriate dashboard

### Session Management
- Supabase handles token storage and refresh
- Session persistence across browser sessions
- Automatic token refresh
- Secure session termination

## Technical Implementation

### User Profile Structure
```typescript
interface Person {
  id: string;
  user_id?: string;        // Supabase auth.users.id
  linkedin_id?: string;    // LinkedIn identifier
  first_name: string;
  last_name: string;
  email?: string;
  companies: {
    company_id: string;
    role_id: string;
  }[];
}

interface AuthSession {
  user: {
    id: string;           // Supabase user id
    person_id: string;    // Reference to people table
    companies: Company[]; // Associated companies
    isCoach: boolean;     // Determined by role
  };
  access_token: string;
}
```

### Security Policies
- Row Level Security (RLS) based on company associations
- Role-based access within companies
- Protected routes in frontend
- Session validation on all API calls

## Success Criteria
- [ ] LinkedIn OAuth integration complete
- [ ] Proper people table integration
- [ ] Company association loading
- [ ] Role-based access control
- [ ] Session management robust

## Dependencies
- Existing people table
- Existing companies table
- people_companies junction table
- LinkedIn OAuth credentials

## Testing Requirements
- OAuth flow testing
- New user creation flow
- Existing user authentication
- Company association loading
- Permission boundary testing 

### Access Flow
1. User authenticates with LinkedIn
2. System checks people table for user
3. If found, check people_companies associations
   - If has company association: proceed to dashboard
   - If no company association: show landing page with message
     "No associated company found. Please contact your coach."
4. If not in people table: show landing page with message
   "No associated company found. Please contact your coach." 