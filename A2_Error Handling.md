**Automating Error Message Validation for Required Fields in a Form (Java and Selenium)**

**Overview**
This demonstrates how to automate error message validation for required fields in a form, specifically for a login page, using Java and Selenium WebDriver. It automates the process of validating the login functionality for a website (SauceDemo) to check if correct error messages appear when required fields (e.g., username and password) are left empty, and whether success messages appear when the correct credentials are entered.

The solution involves creating a Page Object Model (POM) to interact with web elements, using test data from a CSV file, executing the test cases, and generating detailed reports with ExtentReports.

---

**Approach**

**Steps:**
1. **Identify Form Elements:**
   - Username: `By.id("user-name")`
   - Password: `By.id("password")`
   - Login Button: `By.xpath("//input[@id='login-button']")`
   - Error Message: `By.cssSelector("h3[data-test='error']")`
   - Success Message: `By.xpath("//span[@class='title']")`

2. **Create Test Cases:**
   - Valid username and password
   - Invalid username and password
   - Empty username or password
   - Empty username and password

3. **Test Logic:**
   - Input data into the fields using `writeText()` method.
   - Click the login button using `click()` method.
   - Wait for the result using `Thread.sleep()` (Use explicit waits for better synchronization).
   - Validate the error messages using assertions (`assertEquals`).

4. **Report Generation:**
   - Use Log4j for logging test results.
   - Use ExtentReports to generate a detailed HTML report with pass/fail status, expected and actual results, and screenshots for failures.

---

**Steps for Implementation**

1. **Page Object Model (POM):**

   - **LoginPage.java (Page Object Class):**
     - Define locators for the username, password, login button, error messages, and success messages.
     - **Methods:**
       - `Loginwithmsg()`: To input credentials, click login, and check for error or success messages.
       - `msgcheck()`: To compare the expected and actual error/success messages and log the test result.

   ```java
   // Loginwithmsg() method:
   if (isElementDisplayed(Message)) {
       String loginMsg = readText(Message);
       System.out.println("Error Message: " + loginMsg);
   } else if (isElementDisplayed(SuccessElement)) {
       String successMsg = readText(SuccessElement);
       System.out.println("Success Message: " + successMsg);
   } else {
       System.out.println("No message displayed.");
   }

   // Comparing Expected and Actual Error Messages:
   if (isElementDisplayed(Message)) {
       actual_result = readText(Message);
   } else if (isElementDisplayed(SuccessElement)) {
       actual_result = readText(SuccessElement);
   } else {
       System.out.println("No message displayed: " + caseid);
       return this;
   }

   try {
       assertEquals(expected_result, actual_result);
       logger.info("Test Passed for Case ID: " + caseid + " | Expected: " + expected_result + " | Actual: " + actual_result);
   } catch (AssertionError e) {
       logger.error("Test Failed for Case ID: " + caseid + " | Expected: " + expected_result + " | Actual: " + actual_result);
   }
   ```

   - **Getting Actual Error Message:**
   ```java
   if (isElementDisplayed(Message)) {
       return readText(Message);
   } else if (isElementDisplayed(SuccessElement)) {
       return readText(SuccessElement);
   }
   return "No message displayed";
   ```

2. **Test Class:**

   - **LoginPageTest.java (Test Class):**
     - Use `ExtentTest` for detailed logging of each test case.
     - **Test Case Execution:** Read test data from a CSV file, execute the login action, and compare the result using `msgcheck()`.
     - **Result Reporting:** Log the test result (pass/fail) and capture screenshots in case of failure.

   ```java
   // Test case execution
   ExtentTest extentTest = ExtentTestManager.startTest("LoginPageTest", testcase, caseid);
   logger.info("Starting test: " + testcase + " with Case ID: " + caseid);

   loginpage.Loginwithmsg(userData).msgcheck(expected_result, caseid);
   String actual_result = loginpage.getActualResult();
   if (expected_result.equals(actual_result)) {
       logger.info("Test Passed | Case ID: " + caseid);
       extentTest.log(Status.PASS, "Test Passed | Case ID: " + caseid);
   } else {
       logger.error("Test Failed | Case ID: " + caseid);
       extentTest.log(Status.FAIL, "Test Failed | Case ID: " + caseid);
       ExtentTestManager.captureScreenshot(driver, extentTest); // Capture failure screenshot
   }
   ```

3. **Test Data (CSV File):**

   | Case ID | Username       | Password    | Expected Result                                         | Description                        |
   |---------|----------------|-------------|---------------------------------------------------------|------------------------------------|
   | A_L1    | asus           | nepal@12    | Epic sadface: Username and password do not match any user | Login with incorrect username and password |
   | A_L2    | asus2          | secret_sauce| Epic sadface: Username and password do not match any user | Login with incorrect username     |
   | A_L3    | error_user     | nepal@124  | Epic sadface: Username and password do not match any user | Login with incorrect password     |
   | A_L4    | secret_sauce   |             | Epic sadface: Username is required                      | Login with empty username         |
   | A_L5    | error_user     |             | Epic sadface: Password is required                      | Login with empty password         |
   | A_L6    |                |             | Epic sadface: Username is required                      | Login with empty username and password |
   | A_L7    | error_user     | secret_sauce| Products                                                | Login with valid username and password |

   Implement the test logic, where for each test case, provide input (username/password), invoke `Loginwithmsg()`, and use `msgcheck()` to validate error/success messages. Log results based on message comparison (expected vs. actual).

4. **Report Generation:**
   - ExtentReports generates a detailed HTML report for each test case.
   *![Screenshot from 2025-03-25 16-38-27](https://github.com/user-attachments/assets/71605c00-0fcf-44fd-bae5-619b8b72416e)
*
   - Log4j logs detailed information about each test case execution status (pass/fail) with screenshots for failures.
 *![Screenshot from 2025-03-25 16-43-07](https://github.com/user-attachments/assets/1f7bfc58-ec7c-418f-9a9e-61aa207b1447)
*
---

**Conclusion**
By using Page Object Model (POM) and Selenium WebDriver, we can automate the validation of error messages for required fields in a form. The approach uses CSV-based test data, ExtentReports for generating detailed reports, and Log4j for logging test execution results. This structure ensures that error messages like "Username is required" are correctly validated and reported, providing a reliable test automation solution.
