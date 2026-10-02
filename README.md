# PySpark Data Cleaning & CI

A simple PySpark data-processing project that demonstrates **data cleaning, transformation, unit testing, and continuous integration with GitHub Actions**.

The project uses a pure DataFrame transformation function, making the data-processing logic easy to test and maintain.

## Project Structure

```text
spark-project/
├── .github/
│   └── workflows/
│       └── ci.yml
├── pyspark_job.py
├── test_pyspark_job.py
├── requirements.txt
└── README.md
```

## Features

* Clean transaction data using PySpark.
* Remove invalid transaction records.
* Remove records with missing names.
* Remove records with null or non-positive amounts.
* Calculate the amount including 20% tax.
* Unit test the transformation using `pytest`.
* Automatically run tests with GitHub Actions.
* Validate pull requests targeting `develop` and `main`.

## Data Cleaning Logic

The main transformation is implemented in `pyspark_job.py`:

```python
clean_data(df)
```

The function performs the following operations:

1. Removes rows where `amount <= 0`.
2. Removes rows where `name` is `NULL`.
3. Removes rows where `amount` is `NULL`.
4. Adds an `amount_with_tax` column.

The tax calculation uses a 20% tax multiplier:

```text
amount_with_tax = amount × 1.20
```

### Example

Input:

| name     | amount |
| -------- | -----: |
| Alice    |  100.0 |
| Bob      |   50.0 |
| Zero     |    0.0 |
| Negative |  -10.0 |
| NULL     |   75.0 |

After cleaning:

| name  | amount | amount_with_tax |
| ----- | -----: | --------------: |
| Alice |  100.0 |           120.0 |
| Bob   |   50.0 |            60.0 |

The invalid records are removed.

## Requirements

* Python 3.11
* Java 17
* PySpark 3.5.6
* pytest 8.4.2

The exact Python dependencies are defined in `requirements.txt`.

## Installation

Clone the repository:

```bash
git clone <repository-url>
cd spark-project
```

Create and activate a virtual environment:

### Windows

```powershell
python -m venv .venv
.venv\Scripts\activate
```

### Linux / macOS

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Install the dependencies:

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Make sure Java is installed and available:

```bash
java -version
```

Java 17 is recommended for the project and is used by the GitHub Actions workflow.

## Running the Tests

Run all tests with:

```bash
pytest -v
```

The test suite verifies:

* Valid records are preserved.
* Records with zero amounts are removed.
* Records with negative amounts are removed.
* Records with null names are removed.
* Tax calculations are correct.

## Continuous Integration

This project uses **GitHub Actions** for continuous integration.

The workflow is located at:

```text
.github/workflows/ci.yml
```

The CI workflow runs automatically when a pull request is:

* Opened.
* Updated with new commits.
* Reopened.

It applies to pull requests targeting:

```text
develop
main
```

### CI Pipeline

The workflow performs these steps:

```text
Checkout Repository
        ↓
Set Up Java 17
        ↓
Set Up Python 3.11
        ↓
Install Dependencies
        ↓
Run Pytest
        ↓
Pass / Fail
```

This ensures that the test suite passes before changes are merged into the target branch.

## Pull Request Workflow

A typical development workflow is:

```text
Create Feature Branch
        ↓
Implement Changes
        ↓
Add / Update Tests
        ↓
Run Tests Locally
        ↓
Commit Changes
        ↓
Push Feature Branch
        ↓
Create Pull Request
        ↓
GitHub Actions Runs Tests
        ↓
Review
        ↓
Merge into develop/main
```

Example:

```bash
git checkout -b feature/data-cleaning

git add .

git commit -m "Add PySpark data cleaning job and CI tests"

git push origin feature/data-cleaning
```

Then create a Pull Request from:

```text
feature/data-cleaning → develop
```

## Testing Locally Before a Pull Request

It is recommended to run the tests locally before pushing:

```bash
pip install -r requirements.txt
pytest -v
```

If all tests pass, push the changes and create the Pull Request.

## Files Overview

### `pyspark_job.py`

Contains the main PySpark transformation:

```python
clean_data(df)
```

The function follows a DataFrame-in/DataFrame-out approach, keeping the transformation logic isolated and easy to test.

### `test_pyspark_job.py`

Contains unit tests for `clean_data()`.

A session-scoped `SparkSession` fixture is used so that Spark is started once and reused across tests.

### `requirements.txt`

Defines the project's pinned dependencies:

```text
pyspark==3.5.6
pytest==8.4.2
```

### `.github/workflows/ci.yml`

Defines the GitHub Actions CI pipeline responsible for automatically testing pull requests.

## Project Goal

The goal of this project is to demonstrate a basic but production-oriented PySpark workflow that combines:

* **PySpark** for data processing
* **Pytest** for automated testing
* **GitHub Actions** for continuous integration
* **Git branches and pull requests** for controlled development

This structure provides a foundation for expanding the project with more complex transformations, additional tests, data sources, and deployment workflows.
