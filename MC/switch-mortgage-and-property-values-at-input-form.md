# Story Description
We want to switch places of the mortgage Value and Property Value at the Mortgages Comparison input form.**Current order:** Mortgage Value - Property Value - Mortgage Term

**New order:** Property Value - Mortgage Value - Mortgage Term

## Location

components/DaftMortgagesPage/components/MortgageComparisonForm/MortgageComparisonForm.tsx

## Notes

Just had to change the order around

```jsx
<FormBuilder
            initialValues={initialValues}
            formFields={[
              FormFields.MORTGAGE_AMOUNT,
              FormFields.PROPERTY_VALUE,
              FormFields.MORTGAGE_AMOUNT,
              FormFields.MORTGAGE_TERM,
            ]}
          />
```
