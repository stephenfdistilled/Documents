## Purpose of this story

We want to update the color of the lender number as people confuse it with buttons :)

We need to change the color in the following pages:

1. Mortgage Comparison Page
2. Callback page
3. PDP entry point

## Location of code / folders

```markup
URL
http://localhost:3000/daft-mortgages/mortgages-comparison

Folder
Pages / daft-mortgages / mortgage-comparison

Mortgage Comparison table
components / DaftMortgagesPage / OffersTable / OffersTable.tsx

Callback Form
https://www.spstaging.daft.build/daft-mortgages/buying-budget/call-back

but access through the mortgage comparison page and click on a continue button to view, otherwise it will be hidden

PDP
components / widget

```

## Solution

- In the OffersTable just changed the label type from ‘secondary’ to ‘primary’. This changes color from blue to black

```jsx
		<Label
     labelText={lender.toString()}
     labelType={'primary'}
     labelSize={'large'}
      />
```
