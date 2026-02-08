Sunshine Inventory Management: Local SQLite Database Schema for the Sunshine Mobile Ecosystem. Efficiently managing inventory, tracking student distributions, and maintaining an immutable audit trail for the Sunshine mobile application.📌 Core FeaturesHierarchical Inventory: Organized by Category → Product → Stock Variants (Sizes).Transaction Tracking: Real-time logs of which student received what item.Audit Protection: Denormalized log system to preserve history even if items are deleted.Security First: Admin authentication with encrypted password support and security recovery.🗺️ Entity-Relationship Model: The system utilizes a relational structure to ensure data integrity while maintaining flexibility for various product sizes. Code snippeterDiagram
    USERS {
        int id PK
        string username UK
        string password
        string role
        string security_question
        string security_answer
    }

    STUDENTS {
        int id PK
        string name
        string class
        string previous_school
        string section
        string contact
    }

    CATEGORIES {
        int id PK
        string name UK
        string type
    }

    PRODUCTS {
        int id PK
        int category_id FK
        string name
        string description
        string image
    }

    STOCK {
        int id PK
        int product_id FK
        string size
        int quantity
        int min_quantity
    }

    TRANSACTIONS {
        int id PK
        int student_id FK
        int stock_id FK
        int quantity
        date date
        string type
        string note
    }

    STOCK_LOGS {
        int id PK
        string category_name
        string item_name
        string size
        int quantity
        string action
        date date
    }

    CATEGORIES ||--|{ PRODUCTS : "contains"
    PRODUCTS ||--|{ STOCK : "has variants"
    STUDENTS ||--o{ TRANSACTIONS : "receives items"
    STOCK ||--o{ TRANSACTION: "is distributed in."

    
Table, Description, Key Fields
USERS, Manages app access." i d, username, password, security_question"

Table, Description, Key Fields
CATEGORIES, "High-level grouping (e.g., Uniforms"," id, name."
PRODUCTS, "The item definition (e.g., Summer Shirt)." id, category_id, name.
STOCK,The physical inventory + sizing.,"id, product_id, size, quantity"

Table, Description, Key Fields
STUDENT S, Recipientinformationn.  ,"id, name, class, cont.act"
TRANSACTIONS,The bridge between students and stock.,"id, student_id, stock_id, date"
STOCK_L OGS, Immutable history. Uses text fields to survive item deleions.,"category_name, item_name, ac.tion"
