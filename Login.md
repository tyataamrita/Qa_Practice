# Web Automation - Login Functionality

## Introduction
This project automates the login functionality of the SauceDemo website (https://www.saucedemo.com/) using Selenium WebDriver, Java, and TestNG. It implements a structured automation framework following the Page Object Model (POM), with test execution managed through TestNG and reports generated using Extent Reports.

---
##  Understanding the Application (Test Planning)
For login functonality, I selected SauceDemo as the test website which provides a login page for authentication.

After  analyzing website, I identified different test scenarios for login functionality, including valid and invalid credentials. The login page consists of:

- **Username field** (Input)
- **Password field** (Input)
- **Login button** (Action)
- **Error message field** (Validation)
  
## Approach for Automation
To ensure a structured and maintainable automation framework, the following principles are applied:

- **Page Object Model (POM):** Separates test logic from UI interactions.
- **TestNG for Test Execution:** Uses annotations like `@Test`, `@BeforeClass`, and `@AfterClass`.
- **Data-Driven Testing:** Reads test data from a CSV file.
- **Logging & Reporting:** Implements Log4j for logging and Extent Reports for HTML test reports.
- **Screenshot Capture on Failure:** Attaches screenshots to failed test cases.

---

## Steps to Automate Login Functionality

### Step 1: Set Up the Automation Framework

1. Install Java (JDK), Maven, and TestNG.
2. Add the following dependencies in `pom.xml`:

```xml
<dependencies>
    <!-- Selenium -->
    <dependency>
        <groupId>org.seleniumhq.selenium</groupId>
        <artifactId>selenium-java</artifactId>
        <version>4.10.0</version>
    </dependency>

    <!-- TestNG -->
    <dependency>
        <groupId>org.testng</groupId>
        <artifactId>testng</artifactId>
        <version>7.4.0</version>
        <scope>test</scope>
    </dependency>

    <!-- Log4j for logging -->
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-core</artifactId>
        <version>2.14.1</version>
    </dependency>

    <!-- Extent Reports -->
    <dependency>
        <groupId>com.aventstack</groupId>
        <artifactId>extentreports</artifactId>
        <version>5.0.9</version>
    </dependency>
</dependencies>
```

3. Set up WebDriverManager to manage ChromeDriver dynamically.

### Step 2: Implement Page Object Model (POM)
Create a `LoginPage.java` class to encapsulate all login actions.

### Step 3: Write Test Cases

| Case ID | Username     | Password      | Expected Result                                    | Test Case                                  |
|---------|-------------|--------------|---------------------------------------------------|--------------------------------------------|
| A_L1    | asus        | nepal@12     | Epic sadface: Username and password do not match any user | Login with incorrect username and password |
| A_L2    | asus2       | secret_sauce | Epic sadface: Username and password do not match any user | Login with incorrect username              |
| A_L3    | error_user  | nepal@124    | Epic sadface: Username and password do not match any user | Login with incorrect password             |
| A_L4    |             | secret_sauce | Epic sadface: Username is required                | Login with empty username                 |
| A_L5    | error_user  |              | Epic sadface: Password is required                | Login with empty password                 |
| A_L6    |             |              | Epic sadface: Username is required                | Login with empty username and password    |
| A_L7    | error_user  | secret_sauce | Successful                                        | Login with valid username and password    |

### Step 4: Implement Test Scripts
Create a `LoginPageTest.java` class to automate the test cases.

### Step 5: Generate HTML Report

- **Add Extent Reports dependency:**

```xml
<dependency>
    <groupId>com.aventstack</groupId>
    <artifactId>extentreports</artifactId>
    <version>5.0.9</version>
</dependency>
```

- **Initialize Extent Reports in Test Class:**

```java
@BeforeClass
public void beforeClass() {
    ExtentTestManager.initializeExtentReports(this.getClass().getSimpleName());
}
```

- **Logging Test Results:**

```java
String actual_result = loginpage.getActualResult();
if (expected_result.equals(actual_result)) {
    extentTest.log(Status.PASS, "Test Passed | Expected: " + expected_result + " | Actual: " + actual_result);
} else {
    extentTest.log(Status.FAIL, "Test Failed | Expected: " + expected_result + " | Actual: " + actual_result);
    ExtentTestManager.captureScreenshot(driver, extentTest);
}
```

- **Flush Report After Execution:**

```java
@AfterClass
public void afterClass() {
    ExtentManager.extentReports.flush();
}
```

### Where is the HTML Report Stored?
The report is stored in the `extent-reports/` directory with a timestamped filename.

---

## Running the Tests
To execute the tests, run the following command:
```sh
mvn test -Dtest=TestNG
```

After execution, the HTML report can be found in `extent-reports/`.

---

## Authors
- **Amrita Tyata**

