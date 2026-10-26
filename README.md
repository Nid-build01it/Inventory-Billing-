# ShelfSync - Mini Inventory & Billing Management System

A web app for a small retail shop to manage products, customers, billing, inventory and sales history. Built for the **PentaForge Software Developer Internship** technical assignment.

- **Live app:** _coming soon_
- **Demo video:** _coming soon_

---

## Features

Progress against the assignment requirements (tick = working):

- [ ] Dashboard with basic statistics
- [x] Product list
- [x] Add product (with validation)
- [ ] Edit product
- [ ] Delete product (blocked if used on an invoice)
- [ ] Search products
- [ ] Customer management
- [ ] Billing with multiple products and automatic totals
- [ ] Automatic stock update after a sale (overselling blocked)
- [ ] Sales history with invoice details
- [x] Responsive layout (sidebar collapses to a menu button on phones)

> This list is updated as each feature is finished.

## Tech stack

| Part | Choice |
|---|---|
| Backend | Django (Python) |
| Database | SQLite |
| Frontend | HTML, Bootstrap 5, custom CSS, plain JavaScript |
| Hosting | PythonAnywhere (planned) |

## Setup instructions

You need **Python 3** and **Git** installed.

**1. Clone the repository**
```
git clone https://github.com/Nid-build01it/Inventory-Billing-.git
cd Inventory-Billing-
```

**2. Create and activate a virtual environment**

Windows:
```
python -m venv venv
venv\Scripts\activate
```
Mac / Linux:
```
python3 -m venv venv
source venv/bin/activate
```

**3. Install requirements**
```
pip install -r requirements.txt
```

**4. Create the database tables**
```
python manage.py migrate
```

**5. (Optional) Create an admin user**
```
python manage.py createsuperuser
```

**6. Start the server**
```
python manage.py runserver
```

Open http://127.0.0.1:8000/products/ in your browser.
The admin panel is at http://127.0.0.1:8000/admin/.

## Project structure

```
inventory_billing/   project settings and main URLs
shop/                the app
  models.py          database tables
  views.py           page logic
  urls.py            page addresses
  forms.py           product form and validation
  templates/shop/    HTML pages (base layout + pages)
  static/shop/       CSS
manage.py            Django command tool
requirements.txt     Python packages
```

## Database design

| Table | Main columns |
|---|---|
| Product | name, price, stock_quantity, low_stock_threshold, created_at |
| Customer | name, phone, email, created_at _(planned)_ |
| Invoice | customer (optional), created_at, total_amount _(planned)_ |
| InvoiceItem | invoice, product, product_name, quantity, unit_price, line_total _(planned)_ |

Design decisions:
- Money is stored as `DecimalField`, not float, so totals are exact.
- Stock uses `PositiveIntegerField`, so the database never accepts negative stock.
- Invoice items will store a copy of the product name and price at the time of sale, so later price changes never alter old invoices.

## How billing and stock updates will work

1. The server ignores prices sent from the browser and reads them from the database.
2. If the same product appears on two rows, quantities are merged before checking stock.
3. Every product is checked against available stock. If any check fails, nothing is saved and the user sees a clear message.
4. If all checks pass, the invoice, its items and the stock reduction are saved in one **atomic transaction**, so either everything succeeds or nothing does.

## AI usage disclosure

I used Claude (by Anthropic) as a mentor in chat for planning, explaining Django concepts and suggesting code. I typed the code myself, tested it, and committed it to this repository step by step. I used a few AI-suggested snippets as starting points and I am able to explain how each part of the project works.

_(Update this section at the end so it reflects exactly what was done, including any AI help with styling or debugging.)_

## Limitations and future improvements

- No login system (single shop owner assumed)
- SQLite is used for the demo; a production shop would use PostgreSQL
- No PDF invoices, tax or discounts
- No returns or refunds
- Automated tests not yet added