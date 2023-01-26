## Running Tests

```bash
npx playwright test
```

```bash
npx playwright show-report
```

## Advantages of Playwright

### Browser Support

- Can easily test on different browsers at the same time, or target a specific browser.
- Supports all modern browsers including Safari ( Cypress does not support Safari)

### GUI

- Can run headless or Headed (launch the test in a demo GUI or just in command line)
- Headed GUI runs in browser vs Cypress which is a desktop app.

### Auto Wait

- Playwright waits for elements to be actionable prior to performing actions. It also has a rich set of introspection events. The combination of the two eliminates the need for artificial timeouts - the primary cause of flaky tests.

### Tabs

- Supports opening new tabs which Cypress does not

### Auto test generation

- Generates tests by recording actions you take in the browser.

### Trace

- Can save a trace when you run a test with ‘—trace on’ command
- Capture all the information to investigate the test failure. Playwright trace contains test execution screencast, live DOM snapshots, action explorer, test source etc

## Network Events / mocking Api calls

Can work with network requests and api calls with page.route()

However there does not seem to be an easy way to mock API response that is called in getServerSideProps.

When trying to mock a GET request for a users Online Pre Application page.route is used to wait for the relevant endpoint to be called.

In this case the endpoint is  api/v1/users/1619756/mortgages/pre-approval/user-inputs.

We want to do something like this 

```tsx
//Mountebank server is running on port 5001
await page.route(
      'http://localhost:5001/api/v1/users/1619756/mortgages/pre-approval/user-inputs',
      async (route) => {
        const response = await route.fetch(); //Save the OPA inputs 
        const json = await response.json();
        await route.fulfill({ response, json }); //Route is never fulfilled as the endpoint has been called on the server which the test cannot see.
      },
    );
```

I’m looking at Mountebank at the moment to get it to work with Playwright

When navigating to [http://localhost:5001/api/v1/users/1619756/mortgages/pre-approval/user-inputs](http://localhost:5001/api/v1/users/1619756/mortgages/pre-approval/user-inputs) I can see the inputs i need.

But in the Playwright tests no data is returned.
