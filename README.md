# **Automation Testing Project**

This repository contains automated tests for:
- **Login Testing:** Selenium tests for https://www.saucedemo.com/
- **API Testing:** Postman tests for https://www.videogamedb.uk/swagger-ui/index.html#/
- **Performance Testing:** JMeter tests for https://httpbin.org/delay/{seconds_to_delay}

## **1. Project Setup & Installation**
### **1.1 Download Project**
Clone the repository or download the ZIP file:
```sh
git clone git clone https://github.com/tyataamrita/Qa_Practice.git
```
Or download manually:
- Click on **Code** → **Download ZIP**  
- Extract the ZIP file to your desired location.

## **2. Prerequisites**
Ensure you have the following dependencies installed:

| Tool/Library      | Version |
|------------------|---------|
| Java            | 8+      |
| Maven           | Latest  |
| Selenium WebDriver | Latest |
| TestNG          | Latest  |
| JMeter          | 5.4+    |
| Postman         | Latest  |

### **2.1 Install Dependencies via Maven**
Navigate to the `login` folder where the `pom.xml` file is located:
```sh
cd login
mvn clean install
```

## **3. Running Tests**
### **3.1 Login Automation (Selenium)**
To run login tests with error message validation, navigate to the `login` folder:
```sh
cd login
mvn clean test -DsuiteXmlFile=TestNG.xml
```
Alternatively, you can open the `TestNG.xml` file in **Selenium IDE** and run tests manually.

### **3.2 API Testing (Postman)**
1. Open **Postman**.
2. Import the API collection file (`.json`).
3. Set up the test environment.
4. Select the environment in Postman.
5. Run API tests using **Runner**.

### **3.3 Performance Testing (JMeter)**
#### **Run JMeter from CLI**
1. Navigate to the JMeter test directory.
2. Execute the command:
```sh
jmeter -n -t Jmtask.jmx -l results.jtl -e -o report-folder
```
#### **Run JMeter Manually**
1. Open **JMeter GUI**.
2. Load `Jmtask.jmx`.
3. Run the test and analyze the report.

## **4. Test Results**
- **Selenium Test Reports:** Available in `target/surefire-reports/` for emailablehtml report ,extent report  available in extentreport and log report in logs
- **JMeter Performance Reports:** Available in `report-folder/`
- **Postman API Test Results:** Available in the Postman Collection Runner.


---

**Author:** *Amrita*  

```
