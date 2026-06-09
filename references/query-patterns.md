# Mobbin Query Patterns

## Query Formula

```
[product category] + [screen type] + [visible UI components] + [user state/action]
```

Each query should describe what a person would see on screen. Maximum 500 characters. Use `mode: "deep"` for nuanced queries, `mode: "fast"` for quick lookups.

## Platform-Specific Guidance

### iOS Queries
iOS apps use mobile-native patterns. Include these when relevant:
- Bottom sheets, action sheets, half-modals
- Tab bar navigation, swipe gestures
- Pull-to-refresh, floating action buttons
- System-style alerts and permission dialogs
- Full-screen takeovers for onboarding

### Web Queries
Web apps use browser-native patterns. Include these when relevant:
- Sidebar navigation, top navigation bar, breadcrumbs
- Modals, drawers, popovers, dropdown menus
- Multi-column layouts, data tables, dashboards
- Toast notifications, inline validation
- Sticky headers, scroll-based interactions

## Example Queries by Goal

### Onboarding
- "fintech signup screen with phone number input progress indicator security message and continue button"
- "AI app onboarding screen asking user goal with selectable cards and continue button"
- "AI chat welcome screen with suggested prompts and empty conversation state"
- "AI assistant setup screen with permissions integrations and progress steps"
- "SaaS onboarding checklist with completed and pending steps progress bar and skip option"

### Empty States
- "SaaS dashboard empty state with illustration explanatory text primary CTA and secondary link"
- "inbox empty state with illustration and suggested actions"
- "search results empty state with alternative suggestions and filters"

### Error States
- "payment failed screen with error message retry button support link and transaction details"
- "network error screen with retry button offline indicator and cached content"
- "form validation error state with inline error messages and highlighted fields"

### Progress and Pending States
- "transfer pending screen with amount recipient estimated arrival time progress tracker and help link"
- "file upload progress screen with progress bar cancel button and file details"
- "order tracking screen with timeline status steps estimated delivery and contact support"

### Confirmation Screens
- "crypto withdrawal confirmation screen with recipient address network fee warning text and confirm button"
- "money transfer confirmation screen with fee breakdown recipient details and confirm button"
- "subscription upgrade confirmation with plan comparison price change and billing date"

### Success States
- "payment success screen with receipt transaction ID and share button"
- "account created success screen with next steps checklist and primary CTA"
- "transfer completed screen with transaction summary download receipt and done button"

### Settings
- "notification settings screen with grouped toggles channels frequency controls and save button"
- "privacy settings screen with data sharing toggles delete account option and explanation text"
- "security settings screen showing recovery phrase backup option and warning banner"

### Wallet and Crypto
- "crypto wallet seed phrase backup screen with warning text and continue button"
- "wallet recovery phrase confirmation screen with numbered words and validation error"
- "crypto portfolio dashboard with token balances price charts and send receive buttons"

### Fintech Transfers
- "bank transfer pending screen showing amount recipient estimated arrival time and progress status"
- "international transfer screen with exchange rate fee breakdown and delivery estimate"
- "recurring payment setup screen with frequency amount date and recipient fields"

## Flow Query Examples (`search_flows`)

Flow queries describe journeys, not individual screens. Focus on what the user goes through from start to finish.

### Onboarding & Signup
- "user signup flow with email verification profile creation and first-use tutorial"
- "AI app onboarding flow with goal selection permissions and first interaction"
- "SaaS trial onboarding flow with workspace setup team invite and feature tour"

### Checkout & Payment
- "ecommerce checkout flow from cart review through shipping payment to order confirmation"
- "subscription upgrade flow with plan comparison billing entry and confirmation"
- "in-app purchase flow with feature preview pricing and payment"

### Account & Settings
- "account setup flow with profile photo username bio and preferences"
- "two-factor authentication setup flow with method selection and verification"
- "notification preferences flow with channel selection frequency and confirmation"

### Crypto & Fintech
- "crypto wallet creation flow with seed phrase generation backup and verification"
- "money transfer flow from recipient selection through amount entry to confirmation"
- "KYC verification flow with document upload selfie and review status"

### Tips
- Keep flow queries broader than screen queries — describe the journey arc, not UI components
- Use `limit: 3-5` since each flow contains multiple screens
- Combine with `search_screens` to drill into specific steps that need deeper study

## Multi-Batch Research

For broad research topics, use `exclude_screen_ids` to get fresh results across multiple calls:

1. Run the first search with `limit: 20`.
2. Collect the screen IDs from the results.
3. Pass them as `exclude_screen_ids` in the next call with a varied query.
4. Repeat to build a diverse set of references.

This is useful when the user wants comprehensive coverage across many apps rather than the top results for a single query.
