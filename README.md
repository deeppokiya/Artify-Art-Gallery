# 🎨 Artify - The Art Gallery

Artify is a full-stack online art gallery platform built using **ASP.NET Core MVC (.NET 8)**. It enables users to explore, purchase, and manage artworks while providing dedicated dashboards for artists and administrators.

---

## 🚀 Features

### 👤 User

* Browse and search artworks
* Add to wishlist ❤️
* Purchase artworks 💳
* View order history

### 🎨 Artist

* Upload and manage artworks
* Track sales and performance
* Artist dashboard

### 🛠️ Admin

* Manage users, artists, and categories
* Monitor orders and platform activity

---

## 🏗️ Architecture

This project follows a **Layered Architecture**:

* MVC (Presentation Layer)
* Services (Business Logic)
* Repository (Data Access)
* Interfaces (Contracts)
* Models (Entities)

---

## 🛠️ Tech Stack

* ASP.NET Core MVC (.NET 8)
* C#
* PostgreSQL / SQL
* Redis (Caching)
* RabbitMQ (Messaging Queue)
* Elasticsearch (Search Optimization)
* Cloudinary (Image Storage)
* PayPal (Payment Integration)

---

## ⚙️ Configuration Setup

⚠️ **Important:** Sensitive credentials are NOT included in this repository.

Create a file:

```bash
appsettings.Development.json
```

Example structure:

```json
{
  "ConnectionStrings": {
    "pgconn": "YOUR_DATABASE_CONNECTION"
  },
  "Authentication": {
    "Google": {
      "ClientId": "YOUR_CLIENT_ID",
      "ClientSecret": "YOUR_CLIENT_SECRET"
    }
  },
  "Jwt": {
    "Key": "YOUR_SECRET_KEY",
    "Issuer": "YOUR_ISSUER",
    "Audience": "YOUR_AUDIENCE"
  },
  "CloudinarySettings": {
    "CloudName": "",
    "ApiKey": "",
    "ApiSecret": ""
  },
  "EmailSettings": {
    "Email": "",
    "Password": ""
  },
  "PayPal": {
    "ClientId": "",
    "Secret": ""
  },
  "RabbitMQ": {
    "Host": "",
    "Username": "",
    "Password": ""
  }
}
```

---


## 🗄️ Database Schema (PostgreSQL)

Database name: ⁠ artify_db ⁠

### Create Database

⁠ sql
CREATE DATABASE artify_db;
 ⁠

### Tables

#### ⁠ t_user ⁠ — Buyer/General Users

⁠ sql
CREATE TABLE t_user (
    c_user_id        SERIAL PRIMARY KEY,
    c_email          VARCHAR(255) UNIQUE NOT NULL,
    c_password_hash  TEXT,
    c_full_name      VARCHAR(255),
    c_username       VARCHAR(100) UNIQUE,
    c_gender         VARCHAR(20),
    c_mobile         VARCHAR(20),
    c_profile_image  TEXT,
    c_created_at     TIMESTAMP DEFAULT NOW()
);
 ⁠

#### ⁠ t_artist_profile ⁠ — Artist Profiles

⁠ sql
CREATE TABLE t_artist_profile (
    c_artist_id       INT PRIMARY KEY REFERENCES t_user(c_user_id),
    c_artist_name     VARCHAR(255),
    c_artist_email    VARCHAR(255) UNIQUE NOT NULL,
    c_password        TEXT,
    c_biography       TEXT,
    c_cover_image     TEXT,
    c_rating_avg      NUMERIC(3,2) DEFAULT 0,
    c_is_verified     BOOLEAN DEFAULT FALSE,
    c_url             TEXT[],
    c_rejected_count  INT DEFAULT 0,
    c_created_at      TIMESTAMP DEFAULT NOW()
);
 ⁠

#### ⁠ t_category ⁠ — Artwork Categories

⁠ sql
CREATE TABLE t_category (
    c_category_id          SERIAL PRIMARY KEY,
    c_category_name        VARCHAR(100) UNIQUE NOT NULL,
    c_category_description TEXT,
    c_is_active            BOOLEAN DEFAULT TRUE,
    c_created_at           TIMESTAMP DEFAULT NOW()
);
 ⁠

#### ⁠ t_artwork ⁠ — Artworks

⁠ sql
CREATE TABLE t_artwork (
    c_artwork_id      SERIAL PRIMARY KEY,
    c_artist_id       INT REFERENCES t_artist_profile(c_artist_id),
    c_category_id     INT REFERENCES t_category(c_category_id),
    c_title           VARCHAR(255) NOT NULL,
    c_description     TEXT,
    c_price           NUMERIC(12,2) NOT NULL,
    c_preview_path    TEXT,
    c_original_path   TEXT,
    c_approval_status VARCHAR(50) DEFAULT 'Pending',  -- Pending | Approved | Rejected
    c_admin_note      TEXT,
    c_likes_count     INT DEFAULT 0,
    c_sell_count      INT DEFAULT 0,
    c_created_at      TIMESTAMP DEFAULT NOW()
);
 ⁠

#### ⁠ t_order ⁠ — Orders

⁠ sql
CREATE TABLE t_order (
    c_order_id      SERIAL PRIMARY KEY,
    c_buyer_id      INT REFERENCES t_user(c_user_id),
    c_total_amount  NUMERIC(12,2),
    c_order_status  VARCHAR(50) DEFAULT 'Pending',  -- Pending | Completed | Cancelled
    c_created_at    TIMESTAMP DEFAULT NOW()
);
 ⁠

#### ⁠ t_order_item ⁠ — Order Line Items

⁠ sql
CREATE TABLE t_order_item (
    c_order_item_id      SERIAL PRIMARY KEY,
    c_order_id           INT REFERENCES t_order(c_order_id),
    c_artwork_id         INT REFERENCES t_artwork(c_artwork_id),
    c_price_at_purchase  NUMERIC(12,2),
    c_created_at         TIMESTAMP DEFAULT NOW()
);
 ⁠

#### ⁠ t_payment ⁠ — Payments

⁠ sql
CREATE TABLE t_payment (
    c_payment_id             SERIAL PRIMARY KEY,
    c_order_id               INT REFERENCES t_order(c_order_id),
    c_transaction_id         VARCHAR(255),
    c_method                 VARCHAR(50),   -- Card | PayPal | etc.
    c_amount_paid            NUMERIC(12,2),
    c_commission_deducted    NUMERIC(12,2),
    c_artist_payout_amount   NUMERIC(12,2),
    c_payment_status         VARCHAR(50),   -- Paid | Failed | Pending
    c_currency               VARCHAR(10) DEFAULT 'USD',
    c_created_at             TIMESTAMP DEFAULT NOW()
);
 ⁠

#### ⁠ t_wishlist ⁠ — User Wishlists

⁠ sql
CREATE TABLE t_wishlist (
    c_wishlist_id  SERIAL PRIMARY KEY,
    c_user_id      INT REFERENCES t_user(c_user_id),
    c_artwork_id   INT REFERENCES t_artwork(c_artwork_id),
    c_added_at     TIMESTAMP DEFAULT NOW(),
    UNIQUE (c_user_id, c_artwork_id)
);
 ⁠

#### ⁠ t_payout ⁠ — Artist Payouts

⁠ sql
CREATE TABLE t_payout (
    c_payout_id      SERIAL PRIMARY KEY,
    c_artist_id      INT REFERENCES t_artist_profile(c_artist_id),
    c_amount         NUMERIC(12,2),
    c_payout_status  VARCHAR(50) DEFAULT 'Pending',  -- Pending | Approved | Rejected
    c_requested_at   TIMESTAMP DEFAULT NOW(),
    c_processed_at   TIMESTAMP,
    c_admin_note     TEXT
);
 ⁠

#### ⁠ t_admin ⁠ — Admin Users

⁠ sql
CREATE TABLE t_admin (
    c_admin_id      SERIAL PRIMARY KEY,
    c_email         VARCHAR(255) UNIQUE NOT NULL,
    c_password_hash TEXT NOT NULL,
    c_full_name     VARCHAR(255),
    c_created_at    TIMESTAMP DEFAULT NOW()
);


----


## 🧪 Running the Project

### 1. Clone repository

```bash
git clone https://github.com/your-username/Artify-Art-Gallery.git
cd artify-art-gallery
```

### 2. Open solution

Open `.sln` file in Visual Studio

### 3. Configure environment

* Add `appsettings.Development.json`
* Update connection strings

### 4. Run project

* Set MVC project as startup
* Press `F5`

---

## 📂 Project Structure

```
Artify/
│
├── MVC/
├── Repository/
├── Services/
├── Interfaces/
├── Models/
├── API/
└── Artify.sln
```

---

## 🔐 Security Best Practices

* Never commit secrets to GitHub
* Use environment variables or user secrets
* Rotate credentials regularly

---

## 🌟 Future Enhancements

* AI-based artwork recommendation
* Live auction system
* Mobile application
* Stripe/Razorpay integration

---

## 🤝 Contributing

Pull requests are welcome. For major changes, open an issue first.

---

## 📜 License

MIT License

---

## 👨‍💻 Author

Deep Pokiya
GitHub: https://github.com/deeppokiya
