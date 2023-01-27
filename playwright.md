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

### Multiple browsers

- Can specify browsers in config file

```tsx
// playwright.config.ts
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  projects: [
    {
      name: 'chromium',
      use: { ...devices['Desktop Chrome'] },
    },
    {
      name: 'firefox',
      use: { ...devices['Desktop Firefox'] },
    },
    {
      name: 'webkit',
      use: { ...devices['Desktop Safari'] },
    },
  ],
});
```

Or choose from command line

```bash
npx playwright test --project=firefox
```

### Automatic Test Generation

[https://playwright.dev/docs/codegen](https://playwright.dev/docs/codegen)

- Opens 2 windows, you interact with the targeted page on 1, Playwright writes the tests on the other.
- Can specify viewport to test different device sizes
- Emulate a device eg iPhone 11
- Emulate Geolocation, language, timezone

```bash
npx playwright codegen http://localhost:3000/daft-mortgages/online-pre-application/edit
```

### Network

[https://playwright.dev/docs/network](https://playwright.dev/docs/network)

Can block certain file types on requests, eg block images - could improve performance of tests?

```bash
// example.spec.ts
import { test, expect } from '@playwright/test';

test('loads page without images', async ({ page }) => {
  // Block png and jpeg images.
  await page.route(/(png|jpeg)$/, route => route.abort());

  await page.goto('https://playwright.dev');
  // ... test goes here
});
```

### Debugging

[https://playwright.dev/docs/debug](https://playwright.dev/docs/debug)

- Record video replay of test, trace of the steps involved in test, screenshots

```tsx
import { defineConfig } from '@playwright/test';
export default defineConfig({
  use: {
    video: 'on-first-retry',
		screenshot: 'only-on-failure',
		trace: 'retain-on-failure',
  },
});
```

- VS Code - debugger
- Debugger that can be launched from command line

```tsx
npx playwright test --debug
```

### P**arallel** testing

[https://playwright.dev/docs/test-parallel](https://playwright.dev/docs/test-parallel)

- Test run in parallel by default
- Limit max number of test workers in config file or comman line
- 

```tsx
npx playwright test --workers 4
```

```tsx
// playwright.config.ts
import { defineConfig } from '@playwright/test';

export default defineConfig({
  // Limit the number of workers on CI, use default locally
  workers: process.env.CI ? 2 : undefined,
});
```

### Enforce single worker

```tsx
npx playwright test --workers=1
```
