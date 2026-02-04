# Spec Template for Workshop

## Feature: Health Score Calculator

### Context
- Purpose and role in the application: Comprehensive customer health scoring system providing predictive analytics for customer relationship health and churn risk within the Customer Intelligence Dashboard
- How it fits into the larger system: Core business logic module (`lib/healthCalculator.ts`) consumed by CustomerHealthDisplay widget, integrated with CustomerSelector for real-time updates
- Who will use it and when: Dashboard users analyzing customer health to identify at-risk customers and prioritize relationship management efforts

### Requirements
- Functional requirements (what it must do):
  - Calculate customer health scores on 0-100 scale
  - Multi-factor weighted scoring:
    - Payment history (40%): days since last payment, average payment delay, overdue amounts
    - Engagement metrics (30%): login frequency, feature usage count, support tickets
    - Contract status (20%): days until renewal, contract value, recent upgrades
    - Support satisfaction (10%): average resolution time, satisfaction scores, escalation counts
  - Risk level classification:
    - Healthy (71-100): Green indicator
    - Warning (31-70): Yellow indicator
    - Critical (0-30): Red indicator
  - Individual scoring functions for each factor
  - Main `calculateHealthScore` function combining all factors
  - Input validation with descriptive error messages
  - Edge case handling for new customers and missing data

- User interface requirements (CustomerHealthDisplay widget):
  - Overall health score display with color-coded visualization
  - Expandable breakdown showing individual factor scores
  - Loading and error states consistent with dashboard patterns
  - Real-time updates when customer selection changes

- Data requirements:
  - Payment history: `daysSinceLastPayment`, `avgPaymentDelay`, `overdueAmount`
  - Engagement metrics: `loginFrequency`, `featureUsageCount`, `supportTickets`
  - Contract information: `daysUntilRenewal`, `contractValue`, `recentUpgrades`
  - Support data: `avgResolutionTime`, `satisfactionScore`, `escalationCount`

- Integration requirements:
  - Integrates with CustomerSelector for customer data
  - Follows established dashboard component patterns
  - Color coding consistent with other dashboard health indicators

### Constraints
- Technical stack and frameworks: Next.js 15, React 19, TypeScript, Tailwind CSS
- Performance requirements:
  - Efficient algorithms suitable for real-time dashboard updates
  - Minimal computational overhead for dashboard responsiveness
  - Consider caching for repeated calculations
- Design constraints:
  - Pure function architecture with no side effects
  - Responsive design maintaining dashboard layout
  - Consistent loading/error states with other widgets
- File structure and naming conventions:
  - Calculator module: `src/lib/healthCalculator.ts`
  - UI component: `src/components/CustomerHealthDisplay.tsx`
  - Follow camelCase for functions, PascalCase for components
- Props interface and TypeScript definitions:
  ```typescript
  // Data input interfaces
  interface PaymentData {
    daysSinceLastPayment: number;
    avgPaymentDelay: number;
    overdueAmount: number;
  }

  interface EngagementData {
    loginFrequency: number;
    featureUsageCount: number;
    supportTickets: number;
  }

  interface ContractData {
    daysUntilRenewal: number;
    contractValue: number;
    recentUpgrades: boolean;
  }

  interface SupportData {
    avgResolutionTime: number;
    satisfactionScore: number;
    escalationCount: number;
  }

  interface CustomerHealthData {
    payment: PaymentData;
    engagement: EngagementData;
    contract: ContractData;
    support: SupportData;
  }

  // Output interfaces
  type RiskLevel = 'healthy' | 'warning' | 'critical';

  interface HealthScoreBreakdown {
    payment: number;
    engagement: number;
    contract: number;
    support: number;
  }

  interface HealthScoreResult {
    score: number;
    riskLevel: RiskLevel;
    breakdown: HealthScoreBreakdown;
  }

  // Component props
  interface CustomerHealthDisplayProps {
    customerId: string;
    healthData?: CustomerHealthData;
    isLoading?: boolean;
    error?: Error;
  }
  ```
- Security considerations: Validate all numeric inputs; no sensitive data exposure in error messages

### Acceptance Criteria

#### Calculator Functions
- [ ] `calculatePaymentScore` returns 0-100 based on payment history
- [ ] `calculateEngagementScore` returns 0-100 based on engagement metrics
- [ ] `calculateContractScore` returns 0-100 based on contract status
- [ ] `calculateSupportScore` returns 0-100 based on support satisfaction
- [ ] `calculateHealthScore` combines factors with correct weights (40/30/20/10)
- [ ] Returns correct risk level: healthy (71-100), warning (31-70), critical (0-30)
- [ ] All functions are pure with no side effects
- [ ] Input validation throws descriptive errors for invalid data
- [ ] Handles missing/partial data gracefully for new customers

#### UI Component
- [ ] Displays overall health score with color-coded indicator
- [ ] Shows expandable breakdown of individual factor scores
- [ ] Renders loading state while data is fetching
- [ ] Renders error state with user-friendly message
- [ ] Updates in real-time when customer selection changes
- [ ] Responsive design works on mobile and desktop

#### Testing
- [ ] Unit tests cover all calculation functions
- [ ] Edge case tests for boundary conditions (0, 100, thresholds)
- [ ] Tests verify mathematical accuracy of weighted calculations
- [ ] Error handling tests for invalid inputs
- [ ] Realistic customer scenario tests included

#### Documentation
- [ ] JSDoc comments explain business logic and formulas
- [ ] Mathematical rationale documented for weighting scheme
- [ ] Edge case handling documented
