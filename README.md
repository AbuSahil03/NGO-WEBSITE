# NGO-WEBSITE
# NARISAKTI

NARISAKTI is a community website and moderated marketplace that supports women-led enterprises. It includes member accounts, product submissions, customer orders, administrator access, and a MySQL database.

## Features

- Public NGO website with stories, events, gallery, publications, businesses, and videos.
- Member registration, login, profile management, product browsing, search, and order history.
- Seller product submission with product image, name, description, price, and seller story.
- Admin approval workflow for submitted products.
- Approved products automatically appear in the public marketplace.
- Customer ordering with delivery details and COD or UPI payment choice.
- Admin dashboard for seller requests, orders, users, messages, applications, content, gallery, and videos.
- Super administrator access to create and manage other administrator accounts.
- Secure PHP/MySQL backend with uploads, sessions, rate limiting, and CSRF protection.

## Technology

| Area | Technology |
| --- | --- |
| Backend | PHP 8.2+ |
| Database | MySQL 8+ |
| Database access | PDO prepared statements |
| Local server | PHP built-in server |
| Uploads | Local `uploads/` folder |

## Requirements

- PHP 8.2 or newer
- MySQL 8 or newer
- PHP extensions: `pdo_mysql`, `mbstring`, `fileinfo`, `session`, and `json`
- `pdo_sqlite` for importing legacy SQLite data

## Run locally

Open Terminal and run:

```sh
cd "/Users/abusahilislamsardar/Desktop/NARISAKTI-dynamic 2/NARISAKTI-dynamic"
brew services start mysql
php scripts/setup.php
php -S 127.0.0.1:8000 router.php
```

Then open:

- Website: http://127.0.0.1:8000
- Member account: http://127.0.0.1:8000/account.php
- Admin dashboard: http://127.0.0.1:8000/admin/

Press `Control + C` in Terminal to stop the website.

> Do not open `index.html` directly or use a static Live Server extension. PHP must run for the database, login, admin panel, and APIs to work.

## Database setup

Default local MySQL settings:

```text
Host: 127.0.0.1
Port: 3306
Database: narisakti_ngo
User: root
Password: blank
```

If your MySQL setup uses another username or password, create your private config file:

```sh
cp config.example.php config.php
```

Then edit `config.php` with your database details. Never upload or commit `config.php` to GitHub.

To create the first administrator manually:

```sh
ADMIN_EMAIL="admin@yourorganisation.org" \
ADMIN_PASSWORD="choose-a-strong-password" \
php scripts/setup.php
```

The setup command creates all required MySQL tables and imports legacy data once. It does not remove existing tables or records.

## User roles

| Role | Permissions |
| --- | --- |
| Visitor | Browse public website content and approved products |
| Member | Manage profile, browse products, order products, submit products as a seller |
| Administrator | Review products, manage orders, content, enquiries, and applications |
| Super administrator | All admin permissions plus creating and managing admin accounts |

## Marketplace workflow

1. A member logs in and chooses **Become a seller**.
2. The member uploads a product image and enters name, price, description, and seller story.
3. The product is saved as pending and stays private.
4. An administrator reviews the seller request.
5. The administrator can approve, reject, or delete the product.
6. Approved products appear automatically in the public marketplace and search results.
7. Customers select a product, provide order details, and choose COD or UPI.
8. Administrators can view and update orders from the admin dashboard.

## Admin panel

The administrator dashboard includes:

- Seller request approval, rejection, and deletion
- Customer order details and status updates
- Member and administrator account management
- Profile and account settings
- Public website content management
- Gallery and video management
- Contact messages and applications
- Marketplace product moderation
- Visitor and website activity statistics

## Project structure

```text
app/                 PHP application services and API handlers
admin/               Administrator dashboard
api/                 API entry point
database/mysql.sql   MySQL database schema
data/                Legacy import data and local sessions
scripts/             Setup and password recovery scripts
tests/               Integration tests
uploads/             Product images and media uploads
router.php           Local PHP routing
config.example.php   Database configuration template
```

## Password recovery

To reset an administrator password locally:

```sh
php scripts/reset-password.php admin "your-admin-email@example.com"
```

For a member account:

```sh
php scripts/reset-password.php member "member-email@example.com"
```

This invalidates the account’s current sessions.

## Security

- PDO prepared SQL queries
- Password hashing
- CSRF protection
- HTTP-only session cookies
- Same-origin request checks
- Login and sensitive-action rate limiting
- Image and media upload validation
- Protected administrator routes
- Private database configuration file

## Production deployment

Before publishing the website:

- Use HTTPS.
- Use a dedicated MySQL user with a strong password.
- Set `secure_cookies` to `true` in `config.php`.
- Keep `config.php`, `data/`, `app/`, `database/`, `scripts/`, and `tests/` inaccessible from the public web.
- Ensure PHP can write to `uploads/` and `data/php-sessions/`.
- Back up the MySQL database and `uploads/` folder regularly.
- Use Apache or Nginx with PHP-FPM for production; the built-in PHP server is only for local development.

## Legacy files

The active application uses PHP and MySQL. Older Node/SQLite files are retained only for migration or rollback reference and should not be used to run the current website.
