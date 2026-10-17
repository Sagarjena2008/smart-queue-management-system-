# SmartQueue

A responsive queue-management website backed by PHP 8.1+, MySQL/MariaDB, and PDO. Customers can create and look up a token; staff can manage the queue, view daily reports, and export a CSV. Public queue data never includes customer names or phone numbers.

## Run locally with XAMPP

1. Install/start Apache and MySQL in XAMPP. Copy this project folder into `C:\xampp\htdocs\smartqueue`.
2. In phpMyAdmin, import `database\schema.sql`. The script creates the `smartqueue` database and three sample services.
3. Copy `.env.example` to `.env` in the project root. For a default XAMPP install, `DB_USER=root` and an empty `DB_PASSWORD` are correct; set the values to match your local MySQL configuration.
4. Open a terminal in the project root and create the first staff account:

   ```powershell
   C:\xampp\php\php.exe bin\create-admin.php manager "use-a-unique-password-of-12-or-more-characters"
   ```

   Use your own strong password. The account is stored as a PHP password hash; the password is not written into project files.
5. Visit `http://localhost/smartqueue/public/`. Staff sign in at `http://localhost/smartqueue/public/admin.php`.

The PHP `pdo_mysql` extension must be enabled. The public web root is the `public` directory; keep `.env`, `app`, `bin`, and `database` outside the web root. Do not deploy with XAMPP's default empty MySQL password.

## Features

- **Customer:** active service selection, one-click/idempotent token creation, sequential per-service daily token numbers, queue position and wait estimate, same-day token lookup by token number plus registration phone, printable receipt, and five-second live queue refresh.
- **Staff:** password-hashed login, secure session cookies, login throttling, CSRF protection, queue call-next locking, valid status transitions, skip/cancel/recall controls, staff-attributed audit history, search and filters, daily totals, average service duration, peak arrival hour, and authorized CSV export.
- **Privacy and reliability:** prepared statements, validation, MySQL transactions and constraints, no phone numbers in the public display, and clear server-side error logging without returning database details to visitors.

SMS is not connected. No SMS provider credentials or destination are included; the site does not claim notifications were sent. If adding a provider, configure it server-side and obtain customer consent before sending their details.

## Verify the main flows

1. Open the customer page and confirm the three sample services load. Create tokens on two services; token sequences are independent for each service.
2. Print the receipt. Look up its token and phone number; try a wrong phone number and confirm the lookup is rejected.
3. Open the public queue in another tab. Sign in to the staff dashboard, call the next token, then start service and complete it. Confirm the public queue updates without a reload.
4. Test skip, recall, and cancellation from the dashboard. Try an invalid action after a token has completed; the server should reject it.
5. Search by token or customer, apply service/status filters, check the daily report cards, and export CSV while signed in.
6. Sign out, refresh the dashboard, and verify the staff API requires sign-in. Repeated incorrect passwords are rate limited.

For deployment, point Apache's document root at `public`, use HTTPS, set the database connection variables in the host's environment, import the schema, and create staff accounts from a trusted CLI. Keep regular backups of the database and restrict access to exported customer reports.
