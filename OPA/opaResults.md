# Story
As a user, I want to learn whether I am ready to apply for a mortgage or not so that I can start my application process. 

As a user, I want to learn why I am not ready to apply for a mortgage or not so that I can work on those circumstances.

## Task Scope / Description
* Creating url for new page: daft-mortgages/online-mortgage-application/results
* Creating the layout

## Location
pages/daft-mortgages/online-mortgage-application/results/index.tsx
components/Forms/OnlineMortgageApplication/OnlinemortgageApplicationsResultsPage.tsx

## Data for the Checklist Component

returned from components/OnlineMortgageApplicationChecklist/OnlineMortgageAppChecklistConfig
### This logic may change in the future

* loggedInuser - step 1 is green, checkmark icon, step2 is blue
* loggedoutuser - step 1 is blue
* warning - step 1 is red, icon is exclamation mark (user who does not meet criteria to apply for mortgage)

## Sample data

* imported from components/OnlineMortgageApplicationChecklist/OnlineMortgageApplicationResultsConfig
* Mocks three different types of results
* Red Flag (Not eligible to apply eg no work permit / low income etc)
* Yellow Flag (Can apply but with conditions - eg need to wait until pass probation)
* Green Flag ( Can apply, no red or green flag warnings)

## Current Logic
### To view possible results change currentFlagObject to Green/yellow/red on line 38
* Checks the status of the user application (red, yellow or green)
* Displays warnings if yellow or red
* No warnings if Green
* Green and yellow may continue
* Red no option to continue

## Further action
* Needs to be connected to backend instead of sample data




