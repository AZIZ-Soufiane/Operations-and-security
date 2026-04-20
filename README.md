# MySQL Configuration Guide: Workbench & Terminal
This document explains how to configure a dedicated user for Laravel to avoid using the `root` account.

---

## 1. Method using MySQL Workbench (Graphical Interface)
This is the most visual method for managing your users.

1.  **Access settings**: In the "Navigator" panel on the left, click on **Users and Privileges**.
2.  **Create the account**: Click the **Add Account** button at the bottom.
    * **Login Name**: `laravel_app`
    * **Password**: `your_secure_password`
    * Click **Apply**.
3.  **Manage rights (Permissions)**:
    * Go to the **Schema Privileges** tab.
    * Click **Add Entry**.
    * Select **Any Schema** (or choose `coach_flow_db` specifically).
    * Click **OK**.
4.  **Finalization**: Click **Select ALL** to check all rights, then click **Apply**.

---

## 2. Terminal Method (SQL Commands)
This method is faster and used on production servers.

First, connect to MySQL: `mysql -u root -p` then execute these commands:

```sql
-- 1. Create the user with their password
CREATE USER 'laravel_app'@'localhost' IDENTIFIED BY 'your_secure_password';

-- 2. Grant all privileges on all databases
-- Note: Replace *.* with coach_flow_db.* to limit to a single database
GRANT ALL PRIVILEGES ON *.* TO 'laravel_app'@'localhost' WITH GRANT OPTION;

-- 3. Refresh privileges to apply changes
FLUSH PRIVILEGES;

```
## 3. Laravel Project Configuration
```

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=coach_flow_db
DB_USERNAME=laravel_app
DB_PASSWORD=your_secure_password

```

## 4. Troubleshooting: "intl" Extension Error

If the command `php artisan db:show` returns an error regarding the intl extension, follow these steps:

1. Open your php.ini file (Path found via `php --ini`).

2. Search for the line `;extension=intl`.

3. Remove the semicolon (;) at the beginning of the line to enable it.

4. Save and restart your terminal or server (VS Code).

## 5. Final Validation
- To verify that everything is working correctly, run:
```
php artisan db:show

```