# POS System

A Django point-of-sale system with one role-aware login, an Admin Back Office, cashier checkout, branch-level inventory, purchasing, activity history, reports, and saved cashier Z Readings. All records are stored using Django ORM in the local SQLite database (`db.sqlite3`).

## Run locally (Windows)

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
python manage.py migrate
python manage.py runserver
```

Open <http://127.0.0.1:8000/>. The first migration creates a Main Branch and Cash, Card, and Cheque payment methods.

Create the initial administrator with Django's password-hashed account creation command:

```powershell
python manage.py createsuperuser
```

The custom user manager assigns the Admin role automatically to superusers. Sign in at the single login page and use **Maintenance → User** to create Cashier accounts. Select the cashier's branch, choose Cashier, and set a password. The same login routes Admins to Back Office and Cashiers to Cashiering. Use Django's `createsuperuser` command only for the initial administrator; later user accounts should be managed in the application.

## Main workflows

- Add departments, units, suppliers, and products under Maintenance / File.
- Create a Delivery / Purchase to receive stock and record supplier payables.
- Admins can record bad orders and stock adjustments; each stock change creates a movement record.
- Cashiers search or scan product codes, build a cart, process a payment, and print the persisted receipt.
- Cashiers review today's totals and close their shift once with a permanent Z Reading. Completed shifts cannot accept more sales that business day.
- Back Office reports show inventory and movement, sales, payables, activity history, and prior Z Readings. Admins can void a sale before the cashier's day has been closed; stock is restored and the void is recorded.

For deployment, set a private `SECRET_KEY`, `DEBUG=False`, configure `ALLOWED_HOSTS`, serve static assets with the deployment web server, and use HTTPS.
