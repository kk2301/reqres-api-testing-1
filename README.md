# ReqRes API Testing Framework

## 📌 Project Overview

This project demonstrates **API automation testing** using the ReqRes REST API. The framework is built using **Postman, Newman CLI, CSV-based data-driven testing, and GitHub Actions** for Continuous Integration (CI).

The project covers API functional testing, request and response validation, assertions, data-driven testing, HTML reporting, and automated execution through a CI/CD pipeline.

---

## 🎯 Project Objectives

* Automate REST API testing using Postman.
* Execute Postman collections using Newman CLI.
* Perform data-driven API testing using CSV files.
* Validate HTTP status codes and response data.
* Validate request and response fields using JavaScript assertions.
* Generate detailed HTML execution reports.
* Integrate API tests with GitHub Actions.
* Maintain the project using Git and GitHub.

---

## 🛠️ Technologies & Tools

| Technology / Tool          | Purpose                                       |
| -------------------------- | --------------------------------------------- |
| Postman                    | API development and functional testing        |
| Newman                     | Command-line execution of Postman collections |
| Newman HTML Extra Reporter | HTML test reporting                           |
| Node.js                    | Runtime environment for Newman                |
| JavaScript                 | Postman test scripts and assertions           |
| JSON                       | API request and response format               |
| CSV                        | Data-driven test data                         |
| Git                        | Version control                               |
| GitHub                     | Source code repository                        |
| GitHub Actions             | CI/CD automation                              |

---

## 📂 Project Structure

```text
reqres-api-testing/
│
├── .github/
│   └── workflows/
│       └── api-tests.yml
│
├── collections/
│   └── ReqRes API Test.postman_collection.json
│
├── environments/
│   └── ReqRes Dev.postman_environment.json
│
├── testdata_API/
│   ├── Login_Data.csv
│   ├── create_user_data.csv
│   └── put_user_data.csv
│
├── reports/
│   └── report.html
│
├── README.md
│
└── .gitignore
```

### Folder Description

| Folder/File          | Description                                    |
| -------------------- | ---------------------------------------------- |
| `.github/workflows/` | Contains GitHub Actions CI/CD workflow         |
| `collections/`       | Contains Postman API collection                |
| `environments/`      | Contains Postman environment configuration     |
| `testdata_API/`      | Contains CSV files for data-driven API testing |
| `reports/`           | Contains generated HTML execution reports      |
| `README.md`          | Project documentation                          |
| `.gitignore`         | Specifies files that should not be committed   |

---

## 🔗 API Endpoints Covered

The framework covers the following API operations:

| API Operation   | Method | Purpose                  |
| --------------- | ------ | ------------------------ |
| Get All Users   | GET    | Retrieve list of users   |
| Get Single User | GET    | Retrieve a specific user |
| Create User     | POST   | Create a new user        |
| Update User     | PUT    | Update an existing user  |
| Delete User     | DELETE | Delete a user            |

---

## 🧪 Test Coverage

The framework validates:

* HTTP status codes
* Response body
* Response structure
* User ID
* User name
* Job details
* Required response fields
* Data-driven expected values
* API response behavior

### Expected Status Codes

| Request         | Method | Expected Status |
| --------------- | ------ | --------------: |
| Get All Users   | GET    |             200 |
| Get Single User | GET    |             200 |
| Create User     | POST   |             201 |
| Update User     | PUT    |             200 |
| Delete User     | DELETE |             204 |

---

## 📊 Data-Driven API Testing

The framework supports **data-driven testing using CSV files** with Postman and Newman.

Separate CSV files are maintained for different API operations.

### Test Data Files

```text
testdata_API/
├── Login_Data.csv
├── create_user_data.csv
└── put_user_data.csv
```

### 1. Login Test Data

`Login_Data.csv`

Example:

```csv
username,password,scenario,expectedStatus
test@mail.com,123456,Valid Login,200
invalid@mail.com,wrongpass,Invalid Login,400
```

The test uses CSV data to execute multiple login scenarios.

> **Security:** Only dummy/test credentials should be stored in the repository. Never commit real passwords, API keys, authentication tokens, or other secrets.

---

### 2. Create User Test Data

`create_user_data.csv`

Example:

```csv
name,job,expectedStatus
Kunal Kumar,Tester,201
Rahul Kumar,Automation Tester,201
Amit Kumar,Senior QA Engineer,201
```

The POST request uses Postman iteration variables:

```json
{
    "name": "{{name}}",
    "job": "{{job}}"
}
```

The expected status code is also read dynamically from the CSV file.

---

### 3. Update User Test Data

`put_user_data.csv`

Example:

```csv
userId,name,job,expectedStatus
2,Kunal Kumar,Senior Lead Tester,200
3,Rahul Kumar,Automation Tester,200
4,Amit Kumar,QA Engineer,200
```

The PUT request uses:

```text
{{baseURL}}/users/{{userId}}
```

Request body:

```json
{
    "name": "{{name}}",
    "job": "{{job}}"
}
```

The expected status code can be retrieved dynamically using:

```javascript
Number(pm.iterationData.get("expectedStatus"))
```

This allows the same API request to execute with multiple sets of test data.

---

## 🔍 API Assertions

Postman test scripts are used to validate API responses.

Example:

```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```

Response data can also be validated using JavaScript assertions.

Example:

```javascript
const jsonData = pm.response.json();

pm.expect(jsonData).to.have.property("page");
pm.expect(jsonData.data).to.be.an("array");
```

---

## 📋 Prerequisites

Install the following before running the project:

* Node.js
* Postman
* Git
* Newman

### Install Newman

```bash
npm install -g newman
```

### Install Newman HTML Extra Reporter

```bash
npm install -g newman-reporter-htmlextra
```

Verify Newman installation:

```bash
newman --version
```

---

## ▶️ Running the Postman Collection

Run the complete collection using:

```bash
newman run "collections/ReqRes API Test.postman_collection.json" ^
-e "environments/ReqRes Dev.postman_environment.json"
```

---

## 📊 Running Data-Driven Tests

### Create User - POST

```bash
newman run "collections/ReqRes API Test.postman_collection.json" ^
-e "environments/ReqRes Dev.postman_environment.json" ^
--iteration-data "testdata_API/create_user_data.csv"
```

### Update User - PUT

```bash
newman run "collections/ReqRes API Test.postman_collection.json" ^
-e "environments/ReqRes Dev.postman_environment.json" ^
--iteration-data "testdata_API/put_user_data.csv"
```

### Login Testing

```bash
newman run "collections/ReqRes API Test.postman_collection.json" ^
-e "environments/ReqRes Dev.postman_environment.json" ^
--iteration-data "testdata_API/Login_Data.csv"
```

---

## 📄 HTML Test Report

The project uses **Newman HTML Extra Reporter** to generate detailed execution reports.

Run:

```bash
newman run "collections/ReqRes API Test.postman_collection.json" ^
-e "environments/ReqRes Dev.postman_environment.json" ^
-r htmlextra ^
--reporter-htmlextra-export reports/report.html
```

The generated report is available at:

```text
reports/report.html
```

The report provides information such as:

* Total requests
* Passed tests
* Failed tests
* Assertions
* Request details
* Response details
* Response time
* Execution summary

---

## ⚙️ CI/CD - GitHub Actions

The project uses **GitHub Actions** to automatically execute API tests.

The workflow performs the following activities:

1. Checkout the repository.
2. Set up Node.js.
3. Install Newman.
4. Install Newman HTML Extra Reporter.
5. Execute the Postman collection.
6. Generate the HTML report.
7. Upload the report as a GitHub Actions artifact.

### Workflow File

```text
.github/workflows/api-tests.yml
```

This allows API tests to be executed automatically whenever the configured GitHub Actions workflow is triggered.

---

## 🔄 Git Workflow

The project is maintained using Git and GitHub.

### Check Repository Status

```bash
git status
```

### Add Changes

```bash
git add .
```

### Commit Changes

```bash
git commit -m "Update API testing project"
```

### Push Changes

```bash
git push
```

---

## 🚀 Future Enhancements

* Add more negative API scenarios.
* Add authentication and authorization testing.
* Increase data-driven test coverage.
* Add JSON schema validation.
* Add response time validation.
* Improve HTML reporting.
* Add additional API endpoints.
* Enhance GitHub Actions CI/CD pipeline.
* Add scheduled API test execution.
* Add environment-specific test execution.

---

## 👨‍💻 Author

**Kunal Kumar**

QA Automation Engineer

**Skills:**
Manual Testing | Selenium | Java | TestNG | Maven | API Testing | Postman | Newman | SQL | Git | GitHub Actions
