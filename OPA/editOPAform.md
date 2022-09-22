## Story
As a user who has previously submitted the OPA form I want to be able to edit the OPA form so that I can update my data.

* There will be a new GET endpoint to retrieve user details
* There will be new PATCH endpoint to update user details

```
GET -> /api/v1/users/${userId}/mortgages/pre-approval/user-inputs
PATCH -> /v1/users/{user-id}/mortgages/pre-approval

```

## Task Scope & Description
* To create an edit function for a user who has previously submitted an OPA application
* URL:
```
/daft-mortgages/online-pre-application/edit
```

## Files
* pages/daft-mortgages/online-pre-application/edit (edit page for OPA)
* components/forms/buying/budget/buyingbudget/form (Form)
* components/Forms/configs/OnlinePreApplication/PreAppliction (Form Questions and data)

# Edit OPA Form

Created: September 16, 2022 9:50 AM
Last Edited Time: September 16, 2022 4:46 PM
Status: In Review 👀
Type: OPA

## Purpose of this story

Allow users to Update an Online Pre Approval form

## Location of code / folders

```tsx
pages / daft-mortgages / online-pre-application / edit.tsx
componenents / Forms / BuyingBudgetForm / BuyingBudgetForm.tsx
```

## Overview

- User can access OPA edit page by clicking on the ‘edit’ link on the OPA results page
    
    ```tsx
    https://www.daft.ie/daft-mortgages/online-mortgage-application/results
    ```
    
    ![Screenshot 2022-09-16 at 09.59.59.png](Edit%20OPA%20Form%204ad0e414a2ad44e3ac32fb137fd8372d/Screenshot_2022-09-16_at_09.59.59.png)
    
- Force authenticates user, then submits userId in request to get previously completed OPA form
    
    ```tsx
    const token = await authorise(
        {
          ctx,
          redirectBackTo: '/daft-mortgages/online-pre-application',
          forceLogIn: true,
        },
        'daft',
      );
    ```
    
    ```tsx
    const { data = null, status = null } = ctx.query.submit
        ? {}
        : await AccountsAPI.getPreApprovalInputs(
            token,
            ctx.res.locals.user?.user_id ? ctx.res.locals.user?.user_id : 1619756, //1619756 will only be reached by Cypress tests (When a user signs in they will always have a user id, if not signed in they will have been re-directed)
          );
    ```
    
- If a form exists, the inputs are retrieved and populate the form, if none found user is re-directed to create an OPA form
    
    ```tsx
    //if problem with auth or no existing OPA form send to create OPA page
      if (
        status === 401 ||
        (status === 200 && !data) ||
        (status === 404 && ctx.res.locals.user)
      ) {
        res.redirect('/daft-mortgages/online-pre-application');
      }
    
      return { props: { inputs: data, status, token } };
    ```
    
- Format of how user inputs should be do not exactly match what is retrieved from the backend. To get around this an object called Response is created in the correct format.
    
    ```tsx
    //Sample form object in correct format
      const response: any = {
        hasMissedPayments: 'false',
        firstApplicantCitizenship: 'true',
        firstApplicantTaxResident: 'false',
        firstApplicantVisaType: { values: [''] },
        hasAppliedForMortgage: 'false',
        hasAppliedHelpToBuy: 'false',
        hasArrearsHistory: 'false',
        hasDebtIssuesHistory: 'false',
        hasEvidentSavingsRecords: 'false',
        hasEvidentRentRecords: '',
        hasFoundAProperty: 'false',
        hasPotentialRepaymentIssues: 'false',
        isRenting: 'false',
        mortgageAmount: '100000',
        estimatedPurchasePrice: '',
        mortgageApplicationStatus: { values: [''] },
        secondApplicantCitizenship: 'false',
        secondApplicantTaxResident: 'false',
        secondApplicantVisaType: { values: [''] },
      };
    ```
    
- Values from the user’s inputs are then copied into the new Response object
    
    ```tsx
    Object.keys(response).forEach(function (key) {
        //Ensure booleans converted to string as form will not pre-populate boolean data
        if (typeof inputs[key] === 'boolean') {
          inputs[key] = inputs[key].toString();
        }
        //Convert the select values from strings to objects
        if (response[key].values) {
          response[key].values[0] = inputs[key];
        } else {
          response[key] = inputs[key];
        }
      });
    ```
    
- Booleans converted into string as the form will not pre-populate the edit form unless values are strings.
- firstApplicantVisaType, secondApplicantVisaType and mortgageApplicationStatus are returned as single strings from Backend. eg ‘STAMP_1’.
- These need to be converted into {values : ‘STAMP_1’}
- the Response object is then passed to BBForm with the fetched values in the correct format
    
    ```tsx
    <BuyingBudgetForm
            pages={editPreApplicationFormConfig}
            inputs={response}
            formName="preApproval"
            token={token}
          />
    ```
