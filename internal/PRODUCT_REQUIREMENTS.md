# Spendaro Product Requirements Document

## Purpose

The purpose of this document is to outline the requirements for the Spendaro web and mobile application, a personal finance management tool that helps users track their spending, set budgets, and achieve their financial goals by giving every dollar a job.

## Target Audience

The target audience for Spendaro are individual users in a younger demographic (18-40 Y/O) and families, with a heavy focus on the family first experience.

## Product Features

- **Budgeting**: Users should be able to set budgets for different categories, such as food, entertainment, and transportation and items within each. These items consume dollars from an overall dollar amount in the budget following the give every dollar a job paradigm.
- **Tracking**: Users should be able to track their spending by category and see how it compares to their budget.
- **(V2 feature) Goals**: Users should be able to set financial goals and track their progress towards achieving them.
- **(V2 feature) Insights**: Users should be able to get insights into their spending habits and make informed decisions about their finances.
- **(V2 feature) Investing**: Users should be able to track their investments to see how they are performing over time.
- **Security**: Spendaro should use industry-standard security measures to protect user data.
- **Ease of use**: Spendaro should be easy to use and navigate.

> `V1` features are the core features that should be developed first. `V2` features are additional features that can be developed later. `V1` is solely focued on offering a budgeting tool that helps users track their spending and set budgets. `V2` is focued on offering additional features that help users achieve their financial goals overall.

## Technical Requirements

- Platform: Spendaro should be available for web and mobile (iOS / Android)
- Database: Spendaro should use a scalable database (MySQL) to store user data and budget data
- Server: Spendaro should use a server to process user data and all the required features (Go with Echo)

## Data Model

The data model for Spendaro should include the following entities:

1. **User Table**:

```bash
userID: int
username: string
password: string
email: string
firstName: string
lastName: string
dateOfBirth: date
```

**SQL**

```sql
CREATE TABLE User (
    UserID INT PRIMARY KEY AUTO_INCREMENT,
    Username VARCHAR(50) NOT NULL UNIQUE,
    Password VARCHAR(50) NOT NULL,
    Email VARCHAR(50) NOT NULL UNIQUE,
    FirstName VARCHAR(50) NOT NULL,
    LastName VARCHAR(50) NOT NULL,
    DateOfBirth DATE NOT NULL
);
```

2. **Budget**:

```bash
budgetID: int
userID: int
title: string
currentAmount: float
startDate: date
endDate: date
```

**SQL**

```sql
CREATE TABLE Budget (
    BudgetID INT PRIMARY KEY AUTO_INCREMENT,
    UserID INT,
    Title VARCHAR(50) NOT NULL,
    CurrentAmount FLOAT NOT NULL,
    StartDate DATE NOT NULL,
    EndDate DATE NOT NULL,
    FOREIGN KEY (UserID) REFERENCES User(UserID)
);
```

3. **Category**:

```bash
categoryID: int
budgetID: int
budgetName: string
plannedAmount: float
```

**SQL**

```sql
CREATE TABLE Category (
    CategoryID INT PRIMARY KEY AUTO_INCREMENT,
    BudgetID INT,
    BudgetName VARCHAR(50) NOT NULL,
    PlannedAmount FLOAT NOT NULL, # Planned amount for the category, this is a unique feature that Spendaro offers to help better correlate the budget to the actual spending.
    FOREIGN KEY (BudgetID) REFERENCES Budget(BudgetID)
);
```

4. **Entry**:

```bash
entryID: int
categoryID: int
name: string
amount: float
date: date
```

**SQL**

```sql
CREATE TABLE Entry (
    EntryID INT PRIMARY KEY AUTO_INCREMENT,
    CategoryID INT,
    Name VARCHAR(50) NOT NULL,
    Amount FLOAT NOT NULL,
    Date DATE NOT NULL,
    FOREIGN KEY (CategoryID) REFERENCES Category(CategoryID)
);
```

With this data model:

- Users can create budgets and define categories within each budget.
- Each category has a planned amount allocated to it, derived from the total amount of the budget.
- Users can add entries to each category to track their expenses or incomes.

> The data model is designed to help users track their spending and set budgets. The `V2` features will require additional entities and relationships. **This model is a starting point and may need to be updated as the product evolves.**

## API Requirements

The Spendaro API should include the following endpoints:

1. **User Endpoints**:

- `POST /api/v1/user`: Create a new user
- `GET /api/v1/user/{userID}`: Get user by ID
- `PUT /api/v1/user/{userID}`: Update user by ID
- `DELETE /api/v1/user/{userID}`: Delete user by ID

2. **Budget Endpoints**:

- `POST /api/v1/budget`: Create a new budget
- `GET /api/v1/budget/{budgetID}`: Get budget by ID
- `PUT /api/v1/budget/{budgetID}`: Update budget by ID
- `DELETE /api/v1/budget/{budgetID}`: Delete budget by ID

3. **Category Endpoints**:

- `POST /api/v1/category`: Create a new category
- `GET /api/v1/category/{categoryID}`: Get category by ID
- `PUT /api/v1/category/{categoryID}`: Update category by ID
- `DELETE /api/v1/category/{categoryID}`: Delete category by ID

4. **Entry Endpoints**:

- `POST /api/v1/entry`: Create a new entry
- `GET /api/v1/entry/{entryID}`: Get entry by ID
- `PUT /api/v1/entry/{entryID}`: Update entry by ID
- `DELETE /api/v1/entry/{entryID}`: Delete entry by ID

5. **Authentication Endpoints**:

- `POST /api/v1/login`: User login
- `POST /api/v1/logout`: User logout

> The API should be designed to support the core features of Spendaro. Additional endpoints may be required for the `V2` features, and there will be a versioning system to manage changes and updates, hence the `/api/v1/` prefix.

## UI / UX Requirements

**UI**

- The Spendaro user interface should be simple and intuitive on web and mobile
- The app should be easy to navigate and use.
- The app should be visually appealing.
- The app should make the experience of creating budgets, categories and items seamless. Adding and subtracting dollars from any and all items should be easy.

**UX**

- The Spendaro user experience should be positive and engaging.
- Users should be able to easily set budgets, track their spending, and achieve their financial goals.
- The app should be helpful and informative.

## Timeline

Spendaro should be developed and released within 6 months.
