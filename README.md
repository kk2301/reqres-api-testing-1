# ReqRes API Testing Framework

## Project Overview

This project demonstrates API automation testing using the **ReqRes REST API**. It is developed using Postman collections, Newman CLI, and GitHub Actions for Continuous Integration (CI).

The framework supports API functional testing, assertions, data-driven testing using CSV files, automated HTML reporting, and CI/CD execution through GitHub Actions.

---

## Project Objectives

* Automate REST API testing using Postman
* Execute API collections using Newman
* Perform data-driven API testing using CSV files
* Validate HTTP status codes and response data
* Generate detailed HTML execution reports
* Integrate automated execution with GitHub Actions
* Store the project in GitHub for version control

---

## Technologies Used

* Postman
* Newman
* Newman HTML Extra Reporter
* Node.js
* Git
* GitHub
* GitHub Actions
* REST API
* JSON
* JavaScript
* CSV

---

## Project Structure

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

| Folder/File          | Description                                         |
| -------------------- | --------------------------------------------------- |
| `.github/workflows/` | Contains GitHub Actions CI/CD workflow              |
| `collections/`       | Contains Postman API collection                     |
| `environments/`      | Contains Postman environment variables              |
| `testdata_API/`      | Contains CSV files used for data-driven API testing |
| `reports/`           | Contains generated HTML execution reports           |
| `README.md`          | Project documentation                               |
| `.gitignore`         | Specifies files that should not be committed to Git |

---

## API Endpoints Covered

The project automates testing of the following ReqRes APIs:

* GET All Users
* GET Single User
* POST Create User
* PUT Update User
* DELETE User

---

## Test Coverage

| Request         | Method | Expected Status Code | Assertions |
| --------------- | ------ | -------------------: | ---------: |
| Get All Users   | GET    |                  200 |          3 |
| Get Single User | GET    |                  200 |          3 |
| Create User     | POST   |                  201 |          3 |
| Update User     | PUT    |                  200 |          3 |
| Delete User     | DELETE |                  204 |          2 |

**Total: 5 Requests | 14 Assertions**

The framework validates:

* HTTP status codes
* Response body
* User name
* Job details
* User ID where available
* Response structure
* Data-driven expected values

---

## Data-Driven Testing

The framework supports data-driven testing using **CSV files** with Postman and Newman.

Separate CSV files are maintained for different API operations to keep the test data organized and maintainable.

### Test Data Files

```text
testdata_API/
├── Login_Data.csv
├── create_user_data.csv
└── put_user_data.csv
```

### Login Test Data

Example:

```csv
username,password,scenario,expectedStatus
test@mail.com,123456,Valid Login,200
invalid@mail.com,wrongpass,Invalid Login,400
```

> Use only dummy/test credentials in the repository. Never commit real passwords, API keys, tokens, or other secrets.

### Create User Test Data

`create_user_data.csv`

```csv
name,job,expectedStatus
Kunal Kumar,Tester,201
Rahul Kumar,Automation Tester,201
Amit Kumar,Senior QA Engineer,201
```

The POST request uses iteration variables:

```json
{
    "name": "{{name}}",
    "job": "{{job}}"
}
```

### Update User Test Data

`put_user_data.csv`

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

Expected status codes are dynamically read from the CSV:

```javascript
Number(pm.iterationData.get("expectedStatus"))
```

This allows the same Postman request to execute with multiple sets of test data.

---

## Prerequisites

Install the following software before running the project:

* Node.js
* Postman
* Git
* Newman

### Install Newman

```bash
npm install -g newman
```

### Install HTML Reporter

```bash
npm install -g newman-reporter-htmlextra
```

---

## Running the Collection

Execute the collection using:

```bash
newman run "collections/ReqRes API Test.postman_collection.json" ^
-e "environments/ReqRes Dev.postman_environment.json"
```

---

## Running Data-Driven Tests

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

---

## Generate HTML Report

```bash
newman run "collections/ReqRes API Test.postman_collection.json" ^
-e "environments/ReqRes Dev.postman_environment.json" ^
-r htmlextra ^
--reporter-htmlextra-export reports/report.html
```

---

## CI/CD Pipeline - GitHub Actions

The project uses **GitHub Actions** to automate API test execution.

The workflow performs the following steps:

* Checkout the repository
* Set up Node.js
* Install Newman
* Install Newman HTML Extra Reporter
* Execute the Postman API collection
* Generate the HTML report
* Upload the HTML report as a GitHub Actions artifact

### Workflow File

```text
.github/workflows/api-tests.yml
```

---

## HTML Report

After execution, the report is generated in:

```text
reports/report.html
```

The HTML report contains:

* Total tests
* Passed tests
* Failed tests
* Assertions
* Request details
* Response details
* Response time
* Execution summary

Response time may vary depending on API and network conditions.

---

## Git Workflow

The project is maintained using Git and GitHub.

### Check Status

```bash
git status
```

### Add Changes

```bash
git add README.md testdata_API
```

### Commit Changes

```bash
git commit -m "Update README and add API test data"
```

### Push Changes

```bash
git push
```

---

## Future Enhancements

* Add negative API test scenarios
* Add authentication and authorization testing
* Add more data-driven test cases
* Add schema validation
* Improve API test reporting
* Integrate additional APIs
* Enhance GitHub Actions CI/CD pipeline
* Add scheduled API test execution

---

## Author

**Kunal Kumar**

QA Automation Engineer | Selenium | Java | TestNG | Maven | Postman | API Testing | Newman | GitHub Actions
