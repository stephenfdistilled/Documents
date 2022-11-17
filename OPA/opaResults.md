# Story
As a user, I want to learn whether I am ready to apply for a mortgage or not so that I can start my application process. 

As a user, I want to learn why I am not ready to apply for a mortgage or not so that I can work on those circumstances.

## Task Scope / Description
* Create page daft-mortgages/online-mortgage-application/results
* Creating the layout
* Implement logic to show content based on user results (Green, yellow, red flag)

## Location
pages/daft-mortgages/online-mortgage-application/results/index.tsx
components/Forms/OnlineMortgageApplication/OnlinemortgageApplicationsResultsPage.tsx
components/Forms/OnlineMortgageApplication/components/ResultHeading.tsx
components/OnlineMortgageApplicationChecklist/OnlineMortgageAppChecklistConfig

### Flags
* Results based on Flags returned from user Results
* If any RED Flags user is RED (Not ready to apply)
* If no red and any yellow user is YELLOW (Can Continue)
* If no red or yellow user is GREEN (Can Continue)

### Checklist Data
- Data coming from components/OnlineMortgageApplicationChecklist/OnlineMortgageAppChecklistConfig
* hasOpaChecklist - Green or Yellow user
* noOpaChecklist - Has no OPA form completed or is logged out (Logged out users will only reach Entry Page)
* warning - User Results contains Red Flags

## Results
Coming from endpoint 
```tsx
`api/v1/users/${userId}/mortgages/pre-approval/summary`,

```

Data requested in pages/daft-mortgages/online-mortgage-application/results/index.tsx
```tsx
await AccountsAPI.getPreApprovalResults(token, userToken);

```











