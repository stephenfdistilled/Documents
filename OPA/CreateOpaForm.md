## Story
A User should be able to create a new Online Pre Application Form (OPA) for the first time.

## Task Scope & Description
* To create required pages and input fields for OPA form.
* URL:
```
/daft-mortgages/online-pre-application
```
## Create Form Inputs
* Citizenship details (Joint Or Single Applications)
* Savings
* Your Future Home
* Final Q's

### Link to show/hide rules spreadsheet [here](https://docs.google.com/spreadsheets/d/1HXvIXYFpwMiGJ7cBmQrsYoFyD8QG98_FpaA89-bzBxQ/edit#gid=1930473)


## Overview
* Re-using the Buying Budget form
* Passing either opaFlowConfigJoint or opaFlowConfigSingle to BuyingBudgetForm (opaFlowConfig holds all the q's to be in the form)
* Pass auth token to BBForm to identify if user is signed in.
* Pass name as ‘preApproval’ so BBF can identify type of form.

###### pages/daft-mortgages/index.tsx	
```
<BuyingBudgetForm
   pages={opaFlowConfig}
   token={token}
   formName="preApproval"
 />
```

## File Locations
* pages/daft-mortgages/index.tsx (index page for OPA form)
* components/Forms/BuyingBudgetForm/BuyingBudgetForm.tsx (BB Form)
* components/Forms/configs/OnlinePreApplication/PreAppliction (Config data to populate form)
* api/index.ts (Endpoint is located in this file)

### BB Form
expects:

* formName: string;  ('preApplication' or 'buyingBudget')
* pages (opaFlowConfigJoint or opaFlowConfigSingle) or buyingBudgetFormConfig) 

###### optional
* Inputs (The questions user will be asked)
* token (string that holds user information)

as above:

```
<BuyingBudgetForm
   pages={opaFlowConfig}
   token={token}
   formName="preApproval"
 />
```

## Submitting the Form

* Uses the Generic Form component
* All of the Daft forms are available by importing Forms from api

```
import { Forms } from 'api';
```

* Generic form will call the endpoint - either ('preApplication' or 'buyingBudget')

```
 <GenericForm
          formName={formName}
          initialValues={initialValues}
          onSubmitCallback={onSubmitCallback}
          formApiCall={
             formApiCall={formName === 'preApproval' ? submitOPA : submitBB}
          }
          .......
```

* Function requires inputs, auth token and user-id

```
  const onSubmit = async (formSubmission: FormSubmission) => {
    return Forms.preApproval(formSubmission, token, userDetails.userId);
  };
```

### Pre Approval Form
recieves:

* query (the user answers to form q's) 
* token (if user signed in) 
* userId (user id) 

then posts the data to `/api/v1/users/${userId}/mortgages/pre-approval`

```
const preApproval = async (
  query: any,
  token: string | null,
  userId: string | undefined,
): Promise<any> => {
  const headers = {
    ...setAuthHeaders(token),
    ...baseHeaders,
  };

  const response = await daftApiGateway({
    headers,
    method: 'post',
    url: `/api/v1/users/${userId}/mortgages/pre-approval`,
    data: { ...query, formVersion: 1 },
  });
  const { data, status } = response;
  return { data, status };
};

```

