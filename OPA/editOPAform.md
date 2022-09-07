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


