# Lensa Studio
### Photography Studio Management System

<p align="left">
  <img src="https://img.shields.io/badge/PHP-8.1%2B-777BB4?style=for-the-badge&logo=php&logoColor=white" />
  <img src="https://img.shields.io/badge/MySQL-Database-4479A1?style=for-the-badge&logo=mysql&logoColor=white" />
  <img src="https://img.shields.io/badge/Bootstrap-5.3-7952B3?style=for-the-badge&logo=bootstrap&logoColor=white" />
  <img src="https://img.shields.io/badge/GitHub_Actions-CI-2088FF?style=for-the-badge&logo=githubactions&logoColor=white" />
</p>

**Lensa Studio** is a web-based photography studio management system designed to simplify studio operations, from customer reservations and payment verification to photographer assignments, photo gallery management, and operational reporting.

---

## Overview

Lensa Studio provides an integrated platform for managing photography studio activities through three main user roles: Customer, Admin, and Photographer.

The application uses a layered architecture to separate presentation, application logic, and database operations.

## Features

### Customer
- User registration and authentication.
- Browse photography services and packages.
- Make reservations and select available session schedules.
- Upload manual bank transfer payment proofs.
- Monitor reservation and payment status.
- Access and download authorized photo results.

### Admin
- Operational dashboard.
- Manage photography services and packages.
- Manage customer and photographer accounts.
- Manage reservations and photographer assignments.
- Verify or reject payment submissions.
- Manage photo galleries and visibility.
- Generate operational reports.

### Photographer
- View assigned photography schedules.
- Update photography session status.
- Upload photography results.
- Access only assigned reservation information.

## Technology Stack

| Technology | Purpose |
|---|---|
| PHP 8.1+ | Backend |
| MySQL 8+ / MariaDB | Database |
| PDO | Database Connectivity |
| Bootstrap 5.3 | Frontend Framework |
| HTML5 | Page Structure |
| CSS3 | Interface Styling |
| JavaScript | Frontend Interaction |
| GitHub Actions | Continuous Integration |

## System Architecture

The application uses a three-layer architecture:

1. **Presentation Layer:** Handles the user interface through PHP views, HTML, CSS, and JavaScript.
2. **Application Layer:** Contains controllers, services, validation rules, and application logic.
3. **Data Layer:** Handles database operations through repositories, PDO, and MySQL.

This separation helps organize application responsibilities and makes the codebase easier to maintain.

## Project Structure

```text
lensa-studio/
├── .github/
│   └── workflows/
│       └── ci.yml
├── app/
│   ├── Core/
│   ├── Support/
│   ├── views/
│   ├── bootstrap.php
│   ├── Controllers.php
│   ├── Repositories.php
│   └── Services.php
├── config/
│   └── config.php
├── database/
│   └── schema.sql
├── public/
│   ├── assets/
│   └── index.php
├── storage/
│   └── uploads/
├── tests/
│   └── run_checks.php
├── .env.example
├── .gitignore
└── README.md
```

## Installation

### Requirements

- PHP 8.1 or later.
- MySQL 8+ or MariaDB.
- Apache Web Server.
- PHP extensions: `pdo_mysql` and `fileinfo`.
- Laragon or another compatible local development environment.

### 1. Clone the Repository

```bash
git clone https://github.com/USERNAME/lensa-studio-management-system.git
```

Enter the project directory:

```bash
cd lensa-studio-management-system
```

### 2. Configure Environment

Copy `.env.example` to `.env`.

```powershell
Copy-Item .env.example .env
```

Adjust the database configuration according to your local environment.

### 3. Setup Database

Import the following SQL file using phpMyAdmin or MySQL client:

```text
database/schema.sql
```

The script creates the `fotostudio` database, its tables, relationships, indexes, and demonstration data.

**Important:** The SQL script drops and recreates project tables. Use a dedicated development database and do not execute it on a database containing important data.

### 4. Run the Application

Start Apache and MySQL through Laragon.

Configure the Apache document root to the project's `public/` directory.

Alternatively, access the application through:

```text
http://localhost/lensa-studio/public/
```

Make sure the following directories are writable by PHP:

```text
storage/uploads/payments/
storage/uploads/gallery/
```

### 5. Run Tests

Execute the following command from the project root:

```bash
php tests/run_checks.php
```

To check PHP syntax using PowerShell:

```powershell
Get-ChildItem app,public,config,tests -Recurse -Filter *.php |
ForEach-Object { php -l $_.FullName }
```

## Testing and Continuous Integration

The project includes automated checks through GitHub Actions.

The CI workflow performs:

- PHP environment setup.
- PHP extension verification.
- Syntax validation for PHP files.
- Automated project checks.

The workflow is triggered by pushes to supported branches and pull requests targeting `main` or `develop`.

Local development testing previously recorded:

- PHP syntax checks: Passed.
- Automated checks: 29 passed, 0 failed.
- Required PHP extensions: Available.

## Business Rules and Limitations

The current application follows these operational rules:

- Supports one photography studio room.
- Demonstration operating hours: 09:00–18:00.
- Reservation slots are offered at 30-minute intervals.
- Unpaid reservations hold their schedule for 24 hours.
- Initial payment requires at least 30% of the package price.
- Payments use manual bank transfers, not a payment gateway.
- Photo results are private by default.
- Public gallery visibility requires administrative action.
- Automated email and WhatsApp notifications are not included.
- Automatic refund processing is not available.

## Security Considerations

The application implements several security measures:

- Password hashing.
- PDO prepared statements.
- CSRF protection.
- Server-side role-based access control.
- Private file storage.
- File upload validation.
- Reservation scheduling conflict prevention.

For production deployment, use HTTPS, change demonstration passwords, configure database backups, and establish appropriate photo privacy and retention policies.

---

<p align="center">
  <b>Lensa Studio</b>
  <br>
  Photography Studio Management System
  <br><br>
  <sub>Designed to simplify photography studio operations.</sub>
</p>