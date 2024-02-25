# Billing Management System

A comprehensive solution for managing customer billing, usage tracking, and stakeholder approvals.

## Tech Stack

- **Backend:** Ruby on Rails
- **Database:** PostgreSQL
- **Frontend:** React with Redux

## Features

- **User Authentication:** Secure login using BCrypt
- **Role-based Access Control:**
  - Finance: Generate and send bills
  - Customer Success & Sales: Approve or reject bills
- **Bill Tracking:** Comprehensive action history with documentation and reversibility
- **Customer Management:** Track details, preferences, and API usage
- **Usage Monitoring:** Record and analyze customer API consumption
- **Flexible Billing:** Generate bills based on usage and custom limits

## Database Schema

### Users

| Column Name     | Data Type | Details                   |
| --------------- | --------- | ------------------------- |
| id              | integer   | primary key, not null     |
| name            | string    | not null, indexed, unique |
| email           | string    |                           |
| role            | string    | not null                  |
| password_digest | string    | not null                  |
| session_token   | string    | not null, indexed, unique |

Roles: "Finance", "Customer Success", "Sales"

### Customers

| Column Name          | Data Type | Details                      |
| -------------------- | --------- | ---------------------------- |
| id                   | integer   | primary key, not null        |
| csm_id               | integer   | foreign key (users), indexed |
| name                 | string    |                              |
| billing_address      | string    |                              |
| billing_email        | string    |                              |
| monthly_api_limit    | integer   |                              |
| overage_unit_cost    | float     |                              |
| start_date           | date      |                              |
| end_date             | date      |                              |
| require_csm_approval | boolean   |                              |

### Usage

| Column Name | Data Type | Details                           |
| ----------- | --------- | --------------------------------- |
| id          | integer   | primary key, not null             |
| customer_id | integer   | foreign key (customers), not null |
| month       | integer   |                                   |
| year        | integer   |                                   |
| api_usage   | integer   |                                   |

### Bills

| Column Name       | Data Type | Details                           |
| ----------------- | --------- | --------------------------------- |
| id                | integer   | primary key, not null             |
| customer_id       | integer   | foreign key (customers), not null |
| month             | integer   |                                   |
| year              | integer   |                                   |
| overage_units     | integer   |                                   |
| overage_unit_cost | float     |                                   |
| overage_amount    | float     |                                   |
| status            | string    |                                   |
| created_at        | datetime  |                                   |
| updated_at        | datetime  |                                   |

### Bill Actions

| Column Name    | Data Type | Details                       |
| -------------- | --------- | ----------------------------- |
| id             | integer   | primary key, not null         |
| bill_id        | integer   | foreign key (bills), not null |
| stakeholder_id | integer   | foreign key (users), not null |
| action         | string    |                               |
| comment        | string    |                               |
| created_at     | datetime  | not null                      |
| updated_at     | datetime  | not null                      |

## API Endpoints

### HTML API

- `GET /` - Load React web app

### JSON API

#### Users

- `GET /api/users/:userId`
- `POST /api/users`

#### Sessions

- `POST /api/session`
- `DELETE /api/session`

#### Customers

- `GET /api/customers`
- `GET /api/customers/:customerId`

#### Usage

- `GET /api/usage` (with query parameters)

#### Bills

- `GET /api/bills`
- `GET /api/bills/:billId`
- `POST /api/bills`
- `PATCH /api/bills/:id/approve`
- `PATCH /api/bills/:id/reject`

## Frontend Routes

- `/welcome` - Login and Signup
- `/dashboard` - Main Dashboard with Overage Bill Index
- `/bills/:id` - Bill Details
- `/customers` - Customer List
- `/customers/:id` - Customer Details with Usage

## Additional Notes

- Bill creation or updates automatically generate BillActions for activity logging.
- The `overage_unit_cost` is stored in both Customer and Bill models for flexibility.
- The system supports custom approval workflows based on customer preferences.

## Getting Started

1. Clone the repository
2. Install dependencies:
   ```
   bundle install
   npm install
   ```
3. Set up the database:
   ```
   rails db:create
   rails db:migrate
   ```
4. Start the server:
   ```
   rails s
   ```
5. In a new terminal, start the frontend:
   ```
   npm start
   ```
