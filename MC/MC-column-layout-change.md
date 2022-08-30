## Purpose of this story

We want to change the "Suits" column to "Lender specialises in" and we want to change the order of the columns,


## Location of code / folders

```markup

URL
http://localhost:3000/daft-mortgages/mortgages-comparison

Folders
Pages / daft-mortgages / mortgage-comparison
components / DaftMortgagesPage / OffersTable / OffersTable.tsx (Table Layout)
components / DaftMortgagesPage / OffersTable / OffersTable.styled.tsx (Table style)
components / DaftMortgagesPage / DaftMortgagesPage.consts.tsx (Table Data)

Callback Form
https://www.spstaging.daft.build/daft-mortgages/buying-budget/call-back


```

## Solution

- In the OffersTable change the order of the objects in the 'cells' object.

```jsx
		return {
        cells: [
          {
            mobileHeader: TABLE_HEADERS.LENDER,
            content: (
              <Label
                labelText={lender.toString()}
                labelType={'primary'}
                labelSize={'large'}
              />
            ),
          },
          {
            id: 'broker',
            content: accessViaBrokerOnly && (
              <Label
                labelText={'Broker Only'}
                labelType={'primary'}
                labelSize={'small'}
              />
            ),
          },
          {
            //Lender speciality
            id: 'best-for',
            mobileHeader: TABLE_HEADERS.BEST_FOR,
            content: (
              <MoreText
                data-tracking={TrackingValues.SEE_MORE_BEST_FOR}
                text={bestFor.join(', ')}
                shortText={formatBestForShortText(bestFor)}
              />
            ),
          },
          {
            mobileHeader: TABLE_HEADERS.CASHBACK,
            content: !!cashback ? formatCurrency(cashback) : EmptyCellValue,
          },
          {
            mobileHeader: TABLE_HEADERS.RATES,
            content: formatRate(offer),
          },
          {
            mobileHeader: TABLE_HEADERS.MONTHLY_REPAYMENTS,
            content: <strong>{formatCurrency(monthlyRepayments)}</strong>,
            id: 'monthly-repayments',
          },
```

Also change the order in the DaftMortgagesPage.consts.tsx file under TABLE_HEADERS
