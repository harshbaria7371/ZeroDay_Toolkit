# Playwright Global Authentication

Instead of logging into your application before every single test (which is slow and can cause rate-limiting), you can log in once, save the authentication state (cookies, local storage), and inject it into all other tests.

## 1. Setup the Auth Script
Create a file: `tests/auth.setup.ts`

```typescript
import { test as setup, expect } from '@playwright/test';

const authFile = 'playwright/.auth/user.json';

setup('authenticate', async ({ page }) => {
  await page.goto('https://example.com/login');
  await page.getByLabel('Username').fill('testuser');
  await page.getByLabel('Password').fill('password123');
  await page.getByRole('button', { name: 'Sign in' }).click();

  // Wait until the page receives the cookies/tokens (e.g., successful redirect)
  await expect(page.getByRole('button', { name: 'Logout' })).toBeVisible();

  // Save the state
  await page.context().storageState({ path: authFile });
});
```

## 2. Configure Dependencies in `playwright.config.ts`

```typescript
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  projects: [
    // Setup project
    { name: 'setup', testMatch: /.*\.setup\.ts/ },

    // Main test project
    {
      name: 'chromium',
      use: {
        ...devices['Desktop Chrome'],
        // Use the saved state
        storageState: 'playwright/.auth/user.json',
      },
      // Ensure 'setup' runs before 'chromium'
      dependencies: ['setup'],
    },
  ],
});
```

## 3. Ignoring Auth for Specific Tests
If you have a test that specifically needs to test the login flow, you can override the storage state in that specific file:

```typescript
import { test } from '@playwright/test';

test.use({ storageState: { cookies: [], origins: [] } });

test('login with invalid credentials', async ({ page }) => {
  // Test isolated login flow...
});
```
