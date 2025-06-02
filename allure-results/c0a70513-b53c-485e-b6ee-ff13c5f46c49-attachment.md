# Test info

- Name: User registration test @master @sanity @regression
- Location: C:\Automation\PWFramework\tests\AccountRegistration.spec.ts:42:5

# Error details

```
Error: page.goto: Target page, context or browser has been closed
Call log:
  - navigating to "https://tutorialsninja.com/demo/", waiting until "load"

    at C:\Automation\PWFramework\tests\AccountRegistration.spec.ts:30:14
```

# Test source

```ts
   1 | /**
   2 |  * Test Case: Account Registration
   3 |  * 
   4 |  * Tags: @master @sanity @regression
   5 |  * 
   6 |  * Steps:
   7 |  * 1) Navigate to application URL 
   8 |  * 2) Go to 'My Account' and click 'Register'
   9 |  * 3) Fill in registration details with random data
  10 |  * 4) Agree to Privacy Policy and submit the form
  11 |  * 5) Validate the confirmation message
  12 |  */
  13 |
  14 | import { test, expect } from '@playwright/test';
  15 | import { HomePage } from '../pages/HomePage';
  16 | import { RegistrationPage } from '../pages/RegistrationPage';
  17 | import { RandomDataUtil } from '../utils/randomDataGenerator';
  18 | import { TestConfig } from '../test.config';
  19 |
  20 | let homePage: HomePage;
  21 | let registrationPage: RegistrationPage;
  22 | let config: TestConfig;
  23 |
  24 | // Reusable test hooks
  25 | test.beforeEach(async ({ page }) => {
  26 |   // Initialize configuration
  27 |   config = new TestConfig();
  28 |
  29 |   // Navigate to application URL before each test
> 30 |   await page.goto(config.appUrl);
     |              ^ Error: page.goto: Target page, context or browser has been closed
  31 |
  32 |   // Instantiate page objects
  33 |   homePage = new HomePage(page);
  34 |   registrationPage = new RegistrationPage(page);
  35 | });
  36 |
  37 | test.afterEach(async ({ page }) => {
  38 |   // Optional cleanup step: can be used to log out or close popups if required
  39 |   await page.close();
  40 | });
  41 |
  42 | test('User registration test @master @sanity @regression', async ({ page }) => {
  43 |   // Step 1: Navigate to 'My Account' and click 'Register'
  44 |   await homePage.clickMyAccount();
  45 |   await homePage.clickRegister();
  46 |
  47 |   // Step 2: Fill in registration details using random data utility
  48 |   await registrationPage.setFirstName(RandomDataUtil.getFirstName());
  49 |   await registrationPage.setLastName(RandomDataUtil.getLastName());
  50 |   await registrationPage.setEmail(RandomDataUtil.getEmail());
  51 |   await registrationPage.setTelephone(RandomDataUtil.getPhoneNumber());
  52 |
  53 |   // Step 3: Set password and confirm password
  54 |   const password = RandomDataUtil.getPassword();
  55 |   await registrationPage.setPassword(password);
  56 |   await registrationPage.setConfirmPassword(password);
  57 |
  58 |   // Step 4: Agree to Privacy Policy and submit the form
  59 |   await registrationPage.setPrivacyPolicy();
  60 |   await registrationPage.clickContinue();
  61 |
  62 |   // Step 5: Validate confirmation message
  63 |   const confirmationMsg = await registrationPage.getConfirmationMsg();
  64 |   expect(confirmationMsg).toContain('Your Account Has Been Created!');
  65 |
  66 |   // Optional wait to visually verify the result in demo/testing phase
  67 |   await page.waitForTimeout(3000); 
  68 | });
  69 |
```