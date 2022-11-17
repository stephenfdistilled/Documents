## Story
A user can edit their OPA form.

* Request GET endpoint to retrieve user details
* Submit to PATCH endpoint to update user details

```
GET -> /api/v1/users/${userId}/mortgages/pre-approval/user-inputs
PATCH -> /v1/users/{user-id}/mortgages/pre-approval

```

## Task Scope & Description
* Add online-pre-application/edit page
* Check if user has an OPA form and load inputs
* Allow users to update inputs and submit form

* URL:
```
/daft-mortgages/online-pre-application/edit
```

## Files
* pages/daft-mortgages/online-pre-application/edit (edit page for OPA)
* components/forms/buying/budget/buyingbudget/form (Form)
* components/Forms/configs/OnlinePreApplication/PreAppliction (Form Questions and data)

## Overview

- User can access OPA edit page by clicking on the ‘edit’ link on the OPA results page
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

 ### BE - FE integration issue   
- Inputs received from GET endpoint are returned as a string or boolean
- Inputs need to be:
 * string - 'true'
 * object -  { values: [''] }
- Sample object is created to mock correct format
    
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
    
- Booleans converted into string
- Values with multiple possible values converted to  { values: [''] }
- 
    
    ```tsx
    const renderInputs = () => {
    //Convert form object from the BE into form object we can use on FE

    Object.keys(response).forEach(function (key) {
      const valuesToCopy = checkSession() ? checkSession() : inputs; // Returns session data if exists, else data from endpoint

      if (typeof valuesToCopy[key] === 'boolean') {
        //Ensure booleans converted to string as form will not pre-populate boolean data
        valuesToCopy[key] = valuesToCopy[key].toString();
      }

      if (response[key]?.values) {
        response[key].values[0] = valuesToCopy[key]; //Convert the select values from strings to objects
      } else {
        response[key] = valuesToCopy[key];
      }
    });
    return response;
  };

    ```
    
- Booleans converted into string as the form will not pre-populate the edit form unless values are strings.
- firstApplicantVisaType, secondApplicantVisaType and mortgageApplicationStatus are returned as single strings from Backend. eg ‘STAMP_1’.
- These need to be converted into {values : [‘STAMP_1’]}
- the Response object is then passed to BBForm with the fetched values in the correct format
    
    ```tsx
    <BuyingBudgetForm
            pages={editPreApplicationFormConfig}
            inputs={response}
            formName="preApproval"
            token={token}
          />
    ```
