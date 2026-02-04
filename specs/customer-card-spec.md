# Spec Template for Workshop

## Feature: CustomerCard

### Context
- Purpose and role in the application: Individual customer display component that provides at-a-glance customer information for quick identification within the Customer Intelligence Dashboard
- How it fits into the larger system: Used within the CustomerSelector container component as part of the domain health monitoring integration
- Who will use it and when: Dashboard users viewing customer lists to identify and select customers for health monitoring analysis

### Requirements
- Functional requirements (what it must do):
  - Display customer name prominently
  - Display company name
  - Display health score with color-coded indicator
  - Show customer domains (websites) for health monitoring context
  - Display domain count when customer has multiple domains

- User interface requirements:
  - Clean, card-based visual design
  - Color-coded health indicator:
    - Red (0-30): Poor health score
    - Yellow (31-70): Moderate health score
    - Green (71-100): Good health score
  - Basic responsive design for mobile and desktop

- Data requirements:
  - Uses mock data from `src/data/mock-customers.ts`
  - Customer interface includes optional `domains` array of website URLs
  - Supports customers with 1 or multiple domains

- Integration requirements:
  - Integrates with CustomerSelector container component
  - Consumes Customer interface from mock data module

### Constraints
- Technical stack and frameworks: Next.js 15, React 19, TypeScript, Tailwind CSS
- Performance requirements: Card should render instantly with no visible delay
- Design constraints:
  - Responsive breakpoints for mobile (< 640px) and desktop
  - Card should maintain readable layout at all supported sizes
- File structure and naming conventions:
  - Component file: `src/components/CustomerCard.tsx`
  - Follow PascalCase for component naming
- Props interface and TypeScript definitions:
  ```typescript
  interface Customer {
    id: string;
    name: string;
    company: string;
    healthScore: number;
    domains?: string[];
  }

  interface CustomerCardProps {
    customer: Customer;
  }
  ```
- Security considerations: No user input handling required; display-only component

### Acceptance Criteria
- [ ] Displays customer name and company name correctly
- [ ] Health score is visible with appropriate color coding (red/yellow/green)
- [ ] Domains are displayed when available
- [ ] Domain count badge shows when customer has multiple domains
- [ ] Card renders correctly on mobile (< 640px width)
- [ ] Card renders correctly on desktop
- [ ] Component accepts Customer prop with TypeScript type safety
- [ ] Component handles customers with no domains gracefully
