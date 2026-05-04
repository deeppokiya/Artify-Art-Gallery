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
