# Spec Template for Workshop

## Feature: Market Intelligence Widget

### Context
- Purpose and role in the application: Real-time market sentiment and news analysis widget for customer companies within the Customer Intelligence Dashboard
- How it fits into the larger system: Full-stack feature with API route, service layer, and UI component integrated into the dashboard alongside existing widgets; receives company data from CustomerSelector
- Who will use it and when: Dashboard users analyzing market conditions and news sentiment for selected customers to inform relationship management decisions

### Requirements
- Functional requirements (what it must do):
  - **API Layer** (`/api/market-intelligence/[company]`):
    - Validate and sanitize company name input
    - Return JSON with sentiment, news count, and headlines
    - Include realistic API delay simulation
    - Follow existing customer management API route patterns
  - **Service Layer** (`MarketIntelligenceService`):
    - Implement caching with 10-minute TTL expiration
    - Generate realistic company-specific mock headlines and sentiment
    - Centralized error handling with custom `MarketIntelligenceError` class
    - Pure function implementations for testability
  - **UI Component**:
    - Input field for company name with validation
    - Display market sentiment with color-coded indicators (green/yellow/red)
    - Show news article count and last updated timestamp
    - Display top 3 headlines with source and publication date

- User interface requirements:
  - Match existing widget styling and layout patterns
  - Color-coded sentiment indicators consistent with dashboard (green/yellow/red)
  - Loading and error states consistent with other widgets
  - Same input/button/error display patterns as existing components
  - Consistent spacing, typography, and component structure

- Data requirements:
  - Mock data generation for reliable workshop demonstration
  - Sentiment score/label for company
  - News article count
  - Top 3 headlines with source and publication date
  - Last updated timestamp
  - Caching layer to reduce redundant calculations

- Integration requirements:
  - Integrate into main Dashboard component
  - Receive company name from selected customer data
  - Follow same prop passing and state management patterns
  - Maintain responsive grid layout

### Constraints
- Technical stack and frameworks: Next.js 15 App Router with Route Handlers, React 19, TypeScript, Tailwind CSS
- Performance requirements:
  - 10-minute cache TTL for mock news data
  - Realistic API delay simulation for authentic UX
  - Efficient rendering within dashboard grid
- Design constraints:
  - Responsive breakpoints matching dashboard layout
  - Error boundaries for graceful failure handling
  - Consistent widget sizing with existing components
- File structure and naming conventions:
  - API route: `src/app/api/market-intelligence/[company]/route.ts`
  - Service: `src/services/MarketIntelligenceService.ts`
  - UI component: `src/components/MarketIntelligenceWidget.tsx`
  - Error class: `src/errors/MarketIntelligenceError.ts`
- Props interface and TypeScript definitions:
  ```typescript
  // API Response
  interface MarketIntelligenceResponse {
    company: string;
    sentiment: SentimentData;
    newsCount: number;
    headlines: Headline[];
    lastUpdated: string;
  }

  interface SentimentData {
    score: number;        // 0-100
    label: SentimentLabel;
  }

  type SentimentLabel = 'positive' | 'neutral' | 'negative';

  interface Headline {
    title: string;
    source: string;
    publishedAt: string;
    url?: string;
  }

  // Service Layer
  interface CacheEntry<T> {
    data: T;
    expiresAt: number;
  }

  // UI Component Props
  interface MarketIntelligenceWidgetProps {
    companyName?: string;
    onCompanyChange?: (company: string) => void;
  }

  // Error Class
  class MarketIntelligenceError extends Error {
    constructor(
      message: string,
      public code: string,
      public statusCode: number
    ) {
      super(message);
      this.name = 'MarketIntelligenceError';
    }
  }
  ```
- Security considerations:
  - Company name parameter validation to prevent injection attacks
  - Input sanitization for mock data generation
  - Error message sanitization (no sensitive information leakage)
  - Proper timeout handling

### Acceptance Criteria

#### API Layer
- [ ] Route handler at `/api/market-intelligence/[company]` accepts GET requests
- [ ] Validates company name parameter (non-empty, alphanumeric with spaces)
- [ ] Returns 400 error for invalid company names with sanitized message
- [ ] Returns consistent JSON response format
- [ ] Includes realistic delay simulation (200-500ms)
- [ ] Follows existing API route patterns

#### Service Layer
- [ ] `MarketIntelligenceService` class implements caching with 10-minute TTL
- [ ] Cache returns stored data when not expired
- [ ] Cache fetches fresh data when expired
- [ ] `MarketIntelligenceError` class extends Error with code and statusCode
- [ ] Mock data generates realistic, company-specific headlines
- [ ] Sentiment scores vary appropriately (not always the same)
- [ ] All functions are pure with no side effects

#### UI Component
- [ ] Input field accepts company name with validation feedback
- [ ] Displays sentiment with correct color coding (green=positive, yellow=neutral, red=negative)
- [ ] Shows news count and last updated timestamp
- [ ] Renders top 3 headlines with source and date
- [ ] Loading state displays while fetching data
- [ ] Error state displays user-friendly message on failure
- [ ] Matches existing widget styling patterns

#### Dashboard Integration
- [ ] Widget renders in dashboard grid layout
- [ ] Receives company name from selected customer
- [ ] Updates when customer selection changes
- [ ] Maintains responsive design on mobile and desktop
- [ ] Consistent spacing with other dashboard widgets

#### Testing
- [ ] API route handler tests for valid and invalid inputs
- [ ] Service layer tests for caching behavior
- [ ] Component tests for rendering states (loading, success, error)
- [ ] Integration tests for dashboard composition
