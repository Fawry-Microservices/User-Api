# User-Api
The User API manages admin users, allowing for adding, activating, and deactivating them. It also provides functionality to view a list of all users.

## Entity Relationship Diagram (ERD)

The database schema for the `user-api` consists of two main tables: `USER` and `TRANSACTION`. These tables are structured to manage admin users and their related transactions.

### USER Table
The `USER` table stores information about the admin users and includes the following fields:

- `id` (INT, Primary Key): Auto-incremented unique identifier for each user.
- `username` (VARCHAR, Unique, Not Null): Unique username for the user.
- `email` (VARCHAR, Unique, Not Null): Unique email address associated with the user.
- `password_hash` (VARCHAR, Not Null): Hashed password for user authentication.
- `full_name` (VARCHAR, Not Null): Full name of the user.
- `role` (ENUM('admin', 'superadmin')): The role of the user, either `admin` or `superadmin`.
- `status` (ENUM('active', 'inactive', 'deactivated')): Indicates the user's account status.
- `created_at` (TIMESTAMP, Default: CURRENT_TIMESTAMP): The date and time when the user was created.
- `updated_at` (TIMESTAMP, Default: CURRENT_TIMESTAMP ON UPDATE): The date and time when the user's details were last updated.
- `permissions` (JSON, Optional): Stores additional user-specific permissions in JSON format.

### TRANSACTION Table
The `TRANSACTION` table records financial transactions linked to the users and includes the following fields:

- `id` (INT, Primary Key): Auto-incremented unique identifier for each transaction.
- `user_id` (INT, Foreign Key): Links the transaction to a user from the `USER` table.
- `amount` (DECIMAL): The amount involved in the transaction.
- `type` (VARCHAR): Specifies whether the transaction is a `Deposit` or `Withdraw`.
- `notes` (TEXT): Additional information or notes about the transaction.
- `created_at` (TIMESTAMP, Default: CURRENT_TIMESTAMP): The date and time when the transaction was created.

### Relationships
- One `USER` can have multiple `TRANSACTION` entries, creating a one-to-many relationship between the `USER` and `TRANSACTION` tables.

```mermaid
erDiagram
    USER {
        int id PK "Auto Increment, Primary Key"
        varchar username "Unique, not null"
        varchar email "Unique, not null"
        varchar password_hash "Hashed password, not null"
        varchar full_name "Not null"
        enum role "ENUM('admin', 'superadmin')"
        enum status "ENUM('active', 'inactive', 'deactivated')"
        timestamp created_at "Default: CURRENT_TIMESTAMP"
        timestamp updated_at "Default: CURRENT_TIMESTAMP ON UPDATE"
        json permissions "Optional, stores permissions in JSON"
    }
    
    TRANSACTION {
        int id PK "Auto Increment, Primary Key"
        int user_id FK "Foreign Key to USER"
        decimal amount "Transaction amount"
        varchar type "Deposit or Withdraw"
        text notes "Transaction notes"
        timestamp created_at "Default: CURRENT_TIMESTAMP"
    }
    
    USER ||--o{ TRANSACTION : "has many"
