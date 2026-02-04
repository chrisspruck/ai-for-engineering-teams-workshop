# Spec Template for Workshop

## Feature: CustomerSelector

### Context
- Purpose and role in the application: Main customer selection interface for the Customer Intelligence Dashboard, enabling users to browse, search, and select customers
- How it fits into the larger system: Container component that renders CustomerCard components and manages selection state for the dashboard
- Who will use it and when: Dashboard users who need to quickly find and select customers from a list of 100+ customers for health monitoring analysis

### Requirements
- Functional requirements (what it must do):
  - Display a list of customer cards showing name, company, and health score
  - Provide search/filter functionality by customer name or company
  - Manage visual selection state (highlight selected customer)
  - Persist selection across page interactions
  - Handle 100+ customers efficiently

- User interface requirements:
  - Search input field for filtering customers
  - Scrollable list of CustomerCard components
  - Clear visual indicator for selected customer (highlight state)
  - Responsive layout for mobile and desktop

- Data requirements:
  - Consumes customer data from `src/data/mock-customers.ts`
  - Manages selected customer state internally or via props
  - Filters customer list based on search query

- Integration requirements:
  - Renders CustomerCard components for each customer
  - Exposes selection change callback for parent components
  - Integrates with dashboard layout

### Constraints
- Technical stack and frameworks: Next.js 15, React 19, TypeScript, Tailwind CSS
- Performance requirements:
  - Must handle 100+ customers without noticeable lag
  - Search filtering should feel instant (< 100ms perceived delay)
  - Consider virtualization for large lists if needed
- Design constraints:
  - Responsive breakpoints for mobile (< 640px) and desktop
  - Scrollable container with fixed height or max-height
- File structure and naming conventions:
  - Component file: `src/components/CustomerSelector.tsx`
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

  interface CustomerSelectorProps {
    customers: Customer[];
    selectedCustomerId?: string;
    onSelectCustomer: (customerId: string) => void;
  }
  ```
- Security considerations: Sanitize search input to prevent XSS; no sensitive data exposure

### Acceptance Criteria
- [ ] Displays list of customer cards with name, company, and health score
- [ ] Search input filters customers by name (case-insensitive)
- [ ] Search input filters customers by company (case-insensitive)
- [ ] Selected customer is visually highlighted
- [ ] Selection persists when filtering and clearing search
- [ ] Handles empty search results gracefully (shows "no results" message)
- [ ] Performs well with 100+ customers (no visible lag when scrolling or filtering)
- [ ] Component is responsive on mobile and desktop
- [ ] TypeScript props interface is properly defined and enforced
