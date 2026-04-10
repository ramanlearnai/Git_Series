# Logical Data Model — Pinnacle Financial Services Analytics

This model represents the business entities and relationships independent of any technology or platform. It focuses on **what** the data means, not how it is stored.

```mermaid
erDiagram

    Client {
        string Client_ID "Business identifier"
        string Client_Name
        string Client_Segment
        date Relationship_Start_Date
        string Relationship_Manager
        string Office_Location
        string AUM_Tier
        string Status
    }

    Product {
        string Product_ID "Business identifier"
        string Product_Name
        string Product_Category
        string Fee_Type
        number Fee_Rate_BPS
        date Inception_Date
        string Status
    }

    GL_Account {
        string Account_ID "Business identifier"
        string Account_Name
        string Account_Type
        string Account_Category
        string Account_Subcategory
        boolean Is_Revenue
        boolean Is_Expense
        string Financial_Statement
    }

    Department {
        string Department_ID "Business identifier"
        string Department_Name
        string Cost_Center
        string Department_Head
        string Parent_Department
    }

    Calendar {
        date Date "Business identifier"
        number Year
        number Quarter
        string Quarter_Name
        number Month
        string Month_Name
        number Day_of_Month
        number Day_of_Week
        string Day_Name
        boolean Is_Business_Day
        number Fiscal_Year
        number Fiscal_Quarter
    }

    Revenue {
        number AUM_Balance
        number Fee_Revenue
        number Performance_Fee
        number Total_Revenue
        string Source_System
    }

    Expense {
        number Actual_Amount
        string Vendor_Name
        string Expense_Description
        string Source_System
    }

    Budget {
        number Budget_Amount
        number Forecast_Amount
        string Budget_Version
        string Approved_By
        date Approved_Date
    }

    Client ||--o{ Revenue : "generates"
    Product ||--o{ Revenue : "earns"
    GL_Account ||--o{ Revenue : "posted to"
    Calendar ||--o{ Revenue : "on date"

    Department ||--o{ Expense : "incurs"
    GL_Account ||--o{ Expense : "posted to"
    Calendar ||--o{ Expense : "on date"

    Department ||--o{ Budget : "is allocated"
    GL_Account ||--o{ Budget : "budgeted under"
    Calendar ||--o{ Budget : "for period"
```

## Entity Descriptions

| Entity | Description |
|--------|-------------|
| **Client** | Institutional investors and fund holders managed by the firm (pension funds, endowments, trusts) |
| **Product** | Investment products offered by the firm (equity funds, fixed income, alternatives) |
| **GL Account** | General ledger chart of accounts for financial reporting |
| **Department** | Organizational units within the firm |
| **Calendar** | Business calendar with fiscal year alignment |
| **Revenue** | Fee revenue earned from clients on investment products (management fees, performance fees) |
| **Expense** | Operational costs incurred by departments |
| **Budget** | Planned budget allocations by department and GL account |

## Key Business Rules

- A **Client** can generate multiple **Revenue** records across different products, accounts, and time periods
- A **Product** can earn revenue from multiple clients
- **Expenses** and **Budgets** are tracked by **Department** and **GL Account**, not by Client or Product
- **GL Account** classifies entries as either revenue or expense via the `Is_Revenue` / `Is_Expense` flags
- **Budget** supports versioning (`Budget_Version`) and approval tracking
